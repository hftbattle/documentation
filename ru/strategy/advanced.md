## Продвинутые идеи

Данная стратегия является улучшенной версией [improved strategy](ideas.md).

Главной её особенностью является тот факт, что при каждом обновлении стакана мы поддерживаем заявки по обоим направлениям на сразу нескольких ценовых уровнях с неединичными объемами. Количество уровней задается параметром **MAX_LEVELS**, объем на уровне - **VOLUME**. 

Безусловно, в таком случае стратегия становится слишком рискованной. Так как мы держим большой объем заявок на каждом направлении, мы рискуем потерять много денег при пробоях стакана, вызванных сильным сдвигом цены финансового инструмента. Чтобы предохраниться от последовательных крупных заявок, пробивающих стакан, в стратегии предлагается предусмотреть механизм временного останова торговли. Например, можно каждый раз считать объем удара по стакану, и если этот объем выше, чем некоторое пороговое значение, на время приостанавливать торговлю по данному направлению.

Также очень важно поддерживать интервалы между ценовыми уровнями своих заявок, чтобы иметь возможность:
  - сводиться с небольшой выгодой с заявками в окрестности *middle price*;
  - сводиться с более глубокими (достигающими более далеких ценовых уровней относительно *middle price*) и выгодными заявками;

Размер соответствующих интервалов регулируется при помощи параметра **DIFF_BETWEEN_LEVELS**.


В итоге мы получим следующую базовую версию стратегии:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"
#include <vector>
#include <unordered_set>

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) : 
      volume_(config["VOLUME"].as<Amount>(1)),
      volume_before_our_order_(config["VOLUME_BEFORE_OUR_ORDER"].as<Amount>(3)),
      offset_(config["OFFSET"].as<Price>(24)),
      max_levels_(config["MAX_LEVELS"].as<size_t>(2)),
      offset_between_levels_(config["OFFSET_BETWEEN_LEVELS"].as<Price>(8)) {
    set_max_total_amount(config["MAX_POS"].as<Amount>(50));
  }

  void trading_book_update(const OrderBook& order_book) override {
    auto& orders = order_book.orders();
    auto pos = executed_amount();
    
    for (Dir dir : {BID, ASK}) {
      auto my_prices = orders.orders_by_dir_as_map(dir);
      std::unordered_set<Price> levels_used;
        
      auto cur_offset = offset_;
      Amount accumulated_volume = 0;
      for (size_t idx = 0; idx < order_book.depth(); ++idx) {
        if (levels_used.size() >= max_levels_) {
          break;
        }
        
        auto vol = order_book.volume_by_index(dir, idx);
        auto price = order_book.price_by_index(dir, idx);
        
        vol -= orders.volume(dir, price);
        accumulated_volume += vol;
            
        if (vol < volume_before_our_order_ || orders.volume(dir, price)) {
          continue;
        }
            
        Price target_price = price + dir_sign(dir) * order_book.min_step();
        Price diff = abs(order_book.best_price(opposite_dir(dir)) - target_price);
        if (diff < cur_offset) {
          continue;
        }
            
        if (my_prices.find(target_price) == my_prices.end()) {
          add_limit_order(dir, target_price, volume_);
        }
            
        cur_offset = diff + offset_between_levels_;
        levels_used.insert(target_price);
      }
      for (Order* order : orders.orders_by_dir(dir)) {
        if (levels_used.find(order->price()) == levels_used.end()) {
          delete_order(order);
        }
      }
    }
  }

  void trading_deals_update(std::vector<Deal>&& /*deals*/) override { }

  void execution_report_update(const ExecutionReport& /*execution_report*/) override { }
  
private:
  Amount volume_;
  Amount volume_before_our_order_;
  Price offset_;
  size_t max_levels_;
  Price offset_between_levels_;
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
from py_defs import *
from common_enums import *
        

class Params:
    MIN_VOLUME = 1
    FEE = Decimal(8)


def init(strat, config):
    Params.VOLUME = config.get('VOLUME', 1)
    Params.VOLUME_BEFORE_OUR_ORDER = config.get('VOLUME_BEFORE_OUR_ORDER', 3)
    Params.OFFSET = Decimal(config.get('OFFSET', 24))
    Params.MAX_LEVELS = config.get('MAX_LEVELS', 2)
    Params.OFFSET_BETWEEN_LEVELS = config.get('OFFSET_BETWEEN_LEVELS', 8)

    Params.MAX_POS = config.get('MAX_POS', 50)
    strat.set_max_total_amount(Params.MAX_POS)
  

def trading_book_update(strat, trading_book):
    orders = trading_book.orders()
    pos = strat.executed_amount()
  
    for dir in (BID, ASK):
        my_prices = set(order.price() for order in orders.orders_by_dir(dir))
        levels_used = set()
        
        cur_offset = Params.OFFSET
        accumulated_volume = 0
        for idx in xrange(trading_book.depth()):
            if len(levels_used) >= Params.MAX_LEVELS:
                break
            
            vol = trading_book.volume_by_index(dir, idx)
            price = trading_book.price_by_index(dir, idx)
            
            vol -= orders.volume(dir, price)
            accumulated_volume += vol
            
            if vol < Params.VOLUME_BEFORE_OUR_ORDER or orders.volume(dir, price): 
                continue
            
            target_price = price + dir_sign(dir) * trading_book.min_step()
            diff = abs(trading_book.best_price(opposite_dir(dir)) - target_price)
            if diff < cur_offset:
                continue
            
            if target_price not in my_prices:
                strat.add_limit_order(dir, target_price, Params.VOLUME)
            
            cur_offset = diff + Params.OFFSET_BETWEEN_LEVELS
            levels_used.add(target_price)
            
        for order in orders.orders_by_dir(dir):
            if order.price() not in levels_used:
                strat.delete_order(order)
{%- endcodetabs %}

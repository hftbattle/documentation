## Market data

The package repository contains a data folder with the first several hours of market data (configuration file line "day": "{{ book["data.small_day"] }}") of the trading symbol.

That's enough to:
- make sure your code compiles successfully
- debug your strategy

To check out the core ideas of the strategy you may download data for a complete trading day "{{ book["data.full_day"] }}" from [here]({{ book["contest.local_pack.data.url"] }}).

You may do it manually: just unzip the file into the root directory or use *download.py* script from the repository root.

```bash
./download.py
```

The script will download the file *data.zip* and unzip it into the *data* folder.

To test the strategy on the complete trading day you should change the value of the field `day` to "{{ book["data.full_day"] }}" in the configuration file.

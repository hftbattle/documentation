## Exchange data

The package repository contains a data folder with several hours of market data (line in the configuration file *date* that data of several exchange working hours (the "day": "{{ book["data.small_day"] }}" line in the configuration file of the trading symbol.

That's enough to:
- make sure, your code gets compiled
- debug your strategy

To check out the very idea of the strategy you may download a complete trading dataset "{{ book["data.full_day"] }}" from [here]({{ book["contest.local_pack.data.url"] }}).

You may do it by yourself. Just unzip the file into the root directory or use *download.py* script from the repository root

```bash
./download.py
```

The script will download the file *data.zip* and unzip it into the *data* folder.

To test the strategy on the complete dataset you should change the value of the field “day” "{{ book["data.full_day"] }}" in the configuration file.

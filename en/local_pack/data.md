## Exchange data

The package repository contains a data folder, that has several hours of market data (line in the configuration file *date* that data of several exchange working hours (the "day": "{{ book["data.small_day"] }}" line in the configuration file of the trading symbol.

That's enough to:
- make sure, your code gets compiled
- debug your strategy

To check out the very idea of the strategy you may download a complete trading day data "{{ book["data.full_day"] }}" at [the link]({{ book["contest.local_pack.data.url"] }}).

You may do it yourself, unzip the file into the directory root or with the script *download.py* which is in the root of the repository

```bash
./download.py
```

The script downloads the file *data.zip* and unzips it into the *data* folder.


To test the strategy on with the complete day data you should change the value of the field “day” "{{ book["data.full_day"] }}"  in the configuration file 


# asean

## Processing

Download files from: https://data.aseanstats.org/

Import files after cleaning!  
```bash
# import basic indicators
CREATE TABLE asean_basic_indicators(country varchar, area numeric, pop numeric, pop_rate numeric, pop_density numeric, gdp numeric, gdp_rate numeric, gdp_capita numeric, exports numeric, imports numeric, total_trade numeric, fdi numeric, visitor_arrivals numeric);
COPY asean_basic_indicators FROM '/home/steve/maps/asean/asean_basic_indicators_2022.tsv' DELIMITER E'\t' CSV HEADER;

# import macro indicators
CREATE TABLE asean_macro_indicators(country varchar, inflation_rate numeric, currency varchar, exchange_rate_us numeric, labour_rate numeric, unemployment_rate numeric, exports_to_gdp numeric, imports_to_gdp numeric, total_trade_to_gdp numeric, exports_value_rate numeric, imports_value_rate numeric, total_trade_value_rate numeric, fdi_change numeric, fdi_change_rate numeric);
COPY asean_macro_indicators FROM '/home/steve/maps/asean/asean_macro_indicators_2022.tsv' DELIMITER E'\t' CSV HEADER;
```

Export json for web  
```bash
# export all indicators
psql -d world -t -A -c "SELECT jsonb_agg(row_to_json(asean_indicators)) FROM (SELECT a.country, area, RANK() OVER (ORDER BY area DESC) area_rank, (pop*1000)::int pop, RANK() OVER (ORDER BY pop DESC) pop_rank, pop_rate, RANK() OVER (ORDER BY pop_rate DESC) pop_rate_rank, pop_density, RANK() OVER (ORDER BY pop_density DESC) pop_density_rank, gdp, RANK() OVER (ORDER BY gdp DESC) gdp_rank, gdp_rate, RANK() OVER (ORDER BY gdp_rate DESC) gdp_rate_rank, gdp_capita, RANK() OVER (ORDER BY gdp_capita DESC) gdp_capita_rank, exports, RANK() OVER (ORDER BY exports DESC) exports_rank, imports, RANK() OVER (ORDER BY imports DESC) imports_rank, total_trade, RANK() OVER (ORDER BY total_trade DESC) total_trade_rank, fdi, RANK() OVER (ORDER BY fdi DESC) fdi_rank, visitor_arrivals, RANK() OVER (ORDER BY visitor_arrivals DESC) visitor_arrivals_rank, inflation_rate, RANK() OVER (ORDER BY inflation_rate DESC) inflation_rate_rank, exchange_rate_us, RANK() OVER (ORDER BY exchange_rate_us DESC) exchange_rate_us_rank, labour_rate, RANK() OVER (ORDER BY labour_rate DESC) labour_rate_rank, unemployment_rate, RANK() OVER (ORDER BY unemployment_rate DESC) unemployment_rate_rank, exports_to_gdp, RANK() OVER (ORDER BY exports_to_gdp DESC) exports_to_gdp_rank, imports_to_gdp, RANK() OVER (ORDER BY imports_to_gdp DESC) imports_to_gdp_rank, total_trade_to_gdp, RANK() OVER (ORDER BY total_trade_to_gdp DESC) total_trade_to_gdp_rank, exports_value_rate, RANK() OVER (ORDER BY exports_value_rate DESC) exports_value_rate_rank, imports_value_rate, RANK() OVER (ORDER BY imports_value_rate DESC) imports_value_rate_rank, total_trade_value_rate, RANK() OVER (ORDER BY total_trade_value_rate DESC) total_trade_value_rate_rank, fdi_change, RANK() OVER (ORDER BY fdi_change DESC) fdi_change_rank, fdi_change_rate, RANK() OVER (ORDER BY fdi_change_rate DESC) fdi_change_rate_rank FROM asean_basic_indicators a, asean_macro_indicators b WHERE a.country = b.country ORDER BY country) AS asean_indicators;" > ~/test/json/asean_indicators.json
```

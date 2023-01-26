#!/bin/bash
#Author: J.Chacano

time_date=$(echo "\"time\":\"$(date -u +"%FT%T.000Z")\"")
dssummary=$(/usr/local/bin/dssummary - :AV)
dssummary_2=$(/usr/local/bin/dssummary - :AV | tr -d "," | grep -E "\s[0-9]+\s")
collection=$(echo "$dssummary" | tr -d "," | grep aggregate | awk '{print $5}' | cut -d "." -f 1 | awk '{print "\"erc_collection_rate\":" $1}')
parsing=$(echo "$dssummary" | tr -d "," | grep aggregate | awk '{print $13}' | cut -d "." -f 1 | awk '{print "\"erc_parsing_rate\":" $1}')

for ds in $(/usr/local/bin/dssummary - :AV | tr -d "," | grep -E "\s[0-9]+\s" | tr -s ' ' | grep "w/s" | awk '{print $3}' | tr "\n" " ")
do
        ds_c=$(echo "$dssummary_2" | grep -E "\s$ds\s" | tr -d '(' | tr -s ' ' | awk '{print $6}' | cut -d "." -f 1 | awk '{print "\""'$ds'"\"" ":" $1 ","}')
        c+=$ds_c
done

ds_collection=$(echo "$c" | rev | cut -c2- | rev)

for ds in $(/usr/local/bin/dssummary - :AV | tr -d "," | grep -E "\s[0-9]+\s" | tr -s ' ' | grep "w/s" | awk '{print $3}' | tr "\n" " ")
do
   ds_p=$(echo "$dssummary_2" | grep -E "\s$ds\s" | tr -d '(' | tr -s ' ' | awk '{print $14}' | cut -d "." -f 1 | awk '{print "\""'$ds'"\"" ":" $1 ","}')
        p+=$ds_p
done

ds_parsing=$(echo "$p" | rev | cut -c2- | rev)

echo "{$time_date,$collection,$parsing,\"ds_collection_rate\":{$ds_collection},\"ds_parsing_rate\":{$ds_parsing}}" > /var/log/ERC_EPS_$(date +"%Y-%m-%d_%I-%M_%p").json

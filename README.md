# branch bandwidthd

This is the branch with bandwidthd adaptions

## Postgres query examples
```sql
-- Select total bytes received/sent by ip, protocol
select 
  rx.ip as ip,
  rxk+txk as "total",
  rxk as "received", r_icmp, r_udp, r_tcp, r_http,
  txk as "sent", t_icmp, t_udp, t_tcp, t_http
from 
  (select ip, sum(total) as rxk, sum(icmp) as r_icmp, sum(udp) as r_udp, sum(tcp) as r_tcp, sum(http) as r_http from bd_rx_log group by ip) as rx, 
  (select ip, sum(total) as txk, sum(icmp) as t_icmp, sum(udp) as t_udp, sum(tcp) as t_tcp, sum(http) as t_http from bd_tx_log group by ip) as tx 
where 
  tx.ip=rx.ip 
order by "total" desc;
```

# Wireshark twoo twooo two twoo…

Source: PicoCTF

Tools: Curl, Wireshark

Technique: follow stream

Fields: forensic

- lọc các giao thức để tìm điểm đặc biệt thì thấy DNS có sự lặp lại ở những subdomain trên tên miền [reddshrimpandherring.com](http://reddshrimpandherring.com/) và source và Destination chỉ có : 8.8.8.8 , 192.168.38.104 và 18.217.1.57.
- thử dùng curl để truy cập url này nhưng không có kết quả

```
┌──(kali㉿kali)-[~]
└─$ curl reddshrimpandherring.com           
<html>
        <head>
                <script>
                        var forwardingUrl = "/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=js";
                        var destinationUrl = "/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=meta";
                        var addDetection = true;
                        if (addDetection) {
                                var inIframe = window.self !== window.top;
                                forwardingUrl += "&inIframe=" + inIframe;
                                var inPopUp = (window.opener !== undefined && window.opener !== null && window.opener !== window);
                                forwardingUrl += "&inPopUp=" + inPopUp;
                        }
                        window.location.replace(forwardingUrl);
                </script>
                <noscript>
                        <meta http-equiv="refresh" content="1;url=/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=meta" />
                </noscript>
        </head>
</html>
```

- Nhận thấy sự lặp lại ở những subdomain có thể chứa nội dung cờ nên ta thử lọc các ip.dst của dns và khi lọc  dns && ip.dst==18.217.1.57 ta nhận thấy có một đoạn kí tự trước tên web

```
┌──(kali㉿kali)-[~]
└─$ tshark -nr shark2.pcapng -Y 'dns && ip.src==18.217.1.57'
 1634   9.388061  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0xdf26 No such name A cGljb0NU.reddshrimpandherring.com SOA a.gtld-servers.net
 1636   9.439381  18.217.1.57 → 192.168.38.104 DNS 205 Standard query response 0xa12d No such name A cGljb0NU.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA dns-external-master.amazon.com
 1638   9.490669  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x1dd2 No such name A cGljb0NU.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 2043  11.921106  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x3a30 No such name A RntkbnNf.reddshrimpandherring.com SOA a.gtld-servers.net
 2045  11.971614  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0xec57 No such name A RntkbnNf.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 2047  12.021881  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0xabb9 No such name A RntkbnNf.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 2445  14.553435  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x531d No such name A M3hmMWxf.reddshrimpandherring.com SOA a.gtld-servers.net
 2447  14.604708  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0x3bd6 No such name A M3hmMWxf.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 2671  14.657185  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x9e21 No such name A M3hmMWxf.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3143  16.455193  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x99dd No such name A ZnR3X2Rl.reddshrimpandherring.com SOA a.gtld-servers.net
 3152  16.505490  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0x028b No such name A ZnR3X2Rl.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 3155  16.557035  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x2ee1 No such name A ZnR3X2Rl.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3433  18.288746  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x16f6 No such name A YWRiZWVm.reddshrimpandherring.com SOA a.gtld-servers.net
 3441  18.339039  18.217.1.57 → 192.168.38.104 DNS 205 Standard query response 0xe7cb No such name A YWRiZWVm.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA dns-external-master.amazon.com
 3445  18.391844  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x2a4b No such name A YWRiZWVm.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3972  20.316608  18.217.1.57 → 192.168.38.104 DNS 162 Standard query response 0xbe68 No such name A fQ==.reddshrimpandherring.com SOA a.gtld-servers.net
 3981  20.368147  18.217.1.57 → 192.168.38.104 DNS 199 Standard query response 0xbaee No such name A fQ==.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 3984  20.420054  18.217.1.57 → 192.168.38.104 DNS 180 Standard query response 0x4068 No such name A fQ==.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 4364  22.532034  18.217.1.57 → 192.168.38.104 DNS 162 Standard query response 0xa740 No such name A fQ==.reddshrimpandherring.com SOA a.gtld-servers.net
 4373  22.582748  18.217.1.57 → 192.168.38.104 DNS 199 Standard query response 0x683a No such name A fQ==.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 4376  22.633047  18.217.1.57 → 192.168.38.104 DNS 180 Standard query response 0x7418 No such name A fQ==.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
```

- Tập hợp lại ta đc: `cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ**==**`
- decode base64 ta được flag: picoCTF{dns_3xf1l_ftw_deadbeef}

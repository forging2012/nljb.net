---
title: OpenVPN-时间错误会造成VPN无法正常连接
date: '2014-07-04'
description:
categories:

tags:system

---

OpenVPN -> 时间错误会造成VPN无法正常连接
 
连接openvpn时出现错误提示：
	 
	TLS_ERROR: BIO read tls_read_plaintext error: error:140890B2:
	SSL routines:SSL3_GET_CLIENT_CERTIFICATE:no certificate returned
	TLS Error: TLS object -> incoming plaintext read error
	TLS Error: TLS handshake failed
 
这个似乎是提示系统时间和证书时间不一致，具体解决措施为：

	1.修改vps时间与本地时间一致
	2.重启vps
	3.重新连接openvpn试试
	4.如果依旧不能连接openvpn，可以在vps上重新生成一个新的证书。


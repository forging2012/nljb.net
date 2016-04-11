---
title: SSH--C-压缩传输
date: '2014-07-04'
description:
categories:

tags:system

---

	-C
      
	Requests compression of all data (including stdin, stdout, stderr, and data for forwarded X11 and TCP connections).  The compression algorithm is the
	
	same used by gzip(1), and the “level” can be controlled by the CompressionLevel option for protocol version 1.  Compression is desirable on modem

	lines and other slow connections, but will only slow down things on fast networks.  The default value can be set on a host-by-host basis in the con‐

	figuration files; see the Compression option.

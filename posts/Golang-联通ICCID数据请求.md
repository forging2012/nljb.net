---
title: Golang-联通ICCID数据请求
date: '2014-08-05'
description:
categories:

tags:golang

---

	// ICCID
	func ICCID(iccid string) (string, error) {
		// 上传数据
		resp, e := http.PostForm("http://iservice.10010.com/ehallService/helpCenter/wireless/execute",
			url.Values{
				"iccid":    {iccid},
				"proId":    {"011"},
				"backData": {"noname"},
				"callBack": {"wirelessCard.processData"}})
		if e != nil {
			// 返回空
			return "", e
		}
		// 保证I/O正常关闭
		defer resp.Body.Close()
		log.Println(resp.StatusCode, http.StatusOK)
		// 判断返回状态
		if resp.StatusCode == http.StatusOK {
			// 读取返回的数据
			data, err := ioutil.ReadAll(resp.Body)
			if err != nil {
				// 读取异常,返回错误
				return "", err
			}
			log.Println("-->", string(data))
		}
		return "", nil
	}

---

	func ICCID(iccid string) (string, error) {
		// 上传数据
		req, e := http.NewRequest("POST",
			 "http://iservice.10010.com/ehallService/helpCenter/wireless/execute", nil)
		if e != nil {
			// 返回空
			return "", e
		}
		// 设置 Header
		req.Header.Set("Referer",
			 "http://iservice.10010.com/oftenInfo.html?menuId=000400010006")
		req.Header.Set("User-Agent",
			 "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/31.0.1650.57 Safari/537.36")
		req.Header.Set("X-Requested-With", "XMLHttpRequest")
		req.Header.Set("Origin", "http://iservice.10010.com")
		req.Header.Set("Accept-Language", "zh-CN,zh;q=0.8")
		form := url.Values{
			"iccid":    {iccid},
			"proId":    {"011"},
			"backData": {"noname"},
			"callBack": {"wirelessCard.processData"}}
		//对form进行编码
		req.Body = ioutil.NopCloser(strings.NewReader(form.Encode()))
		// 设置 TimeOut
		DefaultClient := http.Client{
			Transport: &http.Transport{
				Dial: func(netw, addr string) (net.Conn, error) {
					deadline := time.Now().Add(60 * time.Second)
					c, err := net.DialTimeout(netw, addr, time.Second*60)
					if err != nil {
						return nil, err
					}
					c.SetDeadline(deadline)
					return c, nil
				},
			},
		}
		// 执行
		resp, ee := DefaultClient.Do(req)
		if ee != nil {
			// 提交异常,返回错误
			return "", ee
		}
		// 保证I/O正常关闭
		defer resp.Body.Close()
		log.Println(resp.StatusCode, http.StatusOK)
		// 判断返回状态
		//if resp.StatusCode == http.StatusOK {
		// 读取返回的数据
		data, err := ioutil.ReadAll(resp.Body)
		if err != nil {
			// 读取异常,返回错误
			return "", err
		}
		log.Println("-->", string(data))
		//}
		return "", nil
	}


---
title: Golang-读取字符串流-ICCID
date: '2014-07-04'
description:
categories:

tags:golang

---

	// CCID
	if *ccid {
	    v := Com(*device, "AT+CRSM=176,12258,0,0,10\r")
	    d := bytes.NewBuffer(v)
	    for {
		l, e := d.ReadString('\n')
		if e == io.EOF {
		    break
		} else if e != nil {
		    os.Exit(2)
		}
		if strings.Contains(l, "+CRSM") {
		    line := strings.Split(z.Trim(l), ",")
		    id := strings.Trim(z.Trim(line[2]), "\"")
		    fmt.Printf("%c%c%c%c%c%c%c%c%c%c%c%c%c%c%c%c%c%c%c\n",
			(id[1]), (id[0]),
			(id[3]), (id[2]),
			(id[5]), (id[4]),
			(id[7]), (id[6]),
			(id[9]), (id[8]),
			(id[11]), (id[10]),
			(id[13]), (id[12]),
			(id[15]), (id[14]),
			(id[17]), (id[16]),
			(id[19]))
	 
		}
	    }
	}


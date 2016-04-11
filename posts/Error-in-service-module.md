---
title: Error-in-service-module
date: '2014-07-09'
description:
categories:

tags:system

---

	linux-hiuk:~ # cat  /etc/pam.d/login
	#%PAM-1.0
	auth     requisite      pam_nologin.so
	auth     [user_unknown=ignore success=ok ignore=ignore auth_err=die default=bad]pam_securetty.so
	auth     include        common-auth
	account  include        common-account
	password include        common-password
	session  required       pam_loginuid.so
	session  include        common-session
	#session  required      pam_lastlog.so  nowtmp showfailed
	session  optional       pam_mail.so standard
	session  optional       pam_ck_connector.so


---
title: Golang-Get-exit-code
date: '2014-07-04'
description:
categories:

tags:golang

---

	package main
	 
	import "os/exec"
	import "log"
	import "syscall"
	 
	func main() {
	    cmd := exec.Command("git", "blub")
	 
	    if err := cmd.Start(); err != nil {
		log.Fatalf("cmd.Start: %v")
	    }
	 
	    if err := cmd.Wait(); err != nil {
		if exiterr, ok := err.(*exec.ExitError); ok {
		    // The program has exited with an exit code != 0
	 
		    // There is no plattform independent way to retrieve
		    // the exit code, but the following will work on Unix
		    if status, ok := exiterr.Sys().(syscall.WaitStatus); ok {
			log.Printf("Exit Status: %d", status.ExitStatus())
		    }
		} else {
		    log.Fatalf("cmd.Wait: %v", err)
		}
	    }
	}



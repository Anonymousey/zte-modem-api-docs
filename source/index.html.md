---
title: ZTE MC801 API

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  # - shell
  # - javascript

toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for ZTE Web UI Modem
  - name: keywords
    content: ZTE,Web,UI,modem,API,Documentation
  - name: google-site-verification
    content: msBeeCY7-smcfjFEqCkjTVrUUu1bTc1qaqBcyVmVvck
---

# Introduction

ZTE MC801 Unofficial API Documentation. You can use this API to access (GET) and change device properties (POST)

The API oriented around two endpoint, accepts raw request bodies, returns JSON in plain text format, and disregard standard HTTP response code, authentication, and verbs.

## Request and response formats

> Request

```http
POST /goform/goform_get_cmd_process?multi_data=1&isTest=false&cmd=modem_main_state%2Cpin_status%2Cloginfo%2Cnew_version_state%2Ccurrent_upgrade_state%2Cis_mandatory&_=1620080390191 HTTP/1.1
Host: 192.168.0.1
Accept: application/json, text/javascript, */*; q=0.01
Referer: http://192.168.0.1/index.html

cmd=modem_main_state,pin_status,loginfo,new_version_state,current_upgrade_state,is_mandatory
```

> Response

```http
HTTP/1.0 200 OK
Server: GoAhead-Webs
Pragma: no-cache
Cache-control: no-cache
Content-Type: text/plain

{"modem_main_state":"modem_init_complete","pin_status":"0","loginfo":"no","new_version_state":"0","current_upgrade_state":"","is_mandatory":""}
```

In general, the Beeline API uses HTTP POST requests with `x-www-form-urlencoded` arguments and JSON responses in `text/plain` format. 

API authentication is using preliminary request that create authenticated sessions for limited time.

<aside class="warning">
All request must contain header attribute <code>Referer</code> that linked to the device Web UI <code>index.html</code> file.
</aside>

### Multiple attributes

You can pass multiple object keys separated by comma into `cmd` parameter to retrieve multiple object in single request.

eg: `cmd=modem_main_state,pin_status,...`

<aside class="warning">
Passing to much object keys can lead to unresponsive device that required <b>force reboot</b>. 
It's advised to pass multiple object keys from the same group (eg: GSM & LTE Objects Group) to prevent said problem.
</aside>

### Form ID

When requesting a change to some properties using the endpoint `/goform/goform_set_cmd_process`, you required to pass an action **Form ID** as value for `goformId` parameter. 

eg: `goformId=<form_id>`

# Authentication

```http
POST /goform/goform_set_cmd_process HTTP/1.1
Connection: keep-alive
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.0.1
Referer: http://192.168.0.1/index.html
Accept-Encoding: gzip, deflate

isTest=false&goformId=LOGIN&password=<password>
```

> Response
```http
Server: ZTE-Webs
Pragma: no-cache
Cache-control: no-cache
Content-Type: text/html
Set-Cookie: zwsd="728f05b19227ad88a2170d2320d956be";HttpOnly="1"
X-Frame-Options: sameorigin
X-XSS-Protection: 1
```
```json
{
  "result":"0"
}
```

ZTE API didn't support any in-flight authorization, instead it relies on device sessions in header (eg. Cookie: zwsd="1dbcbdaa196026e26e999387e7bc1725) looks like MD5. Not sure how its generated. 

<aside class="notice">
The duration of authenticated session is still <b>unknown!</b> The workaround to that limitation you can schedule recurring AUTH requests or do authentication as preliminary for every request.
</aside>

### Form ID

`LOGIN`

### Parameters

Name|Default|Description
---|---|---
**`password`** | - | Your password in `base64` encoding

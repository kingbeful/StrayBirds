---
layout: default
title: Using luasocket for Http request
comments: true
---

I take [this](http://kracekumar.com/post/55856724724/http-request-examples-for-luasocket) for ref


```lua
local http = require("socket.http")
local ltn12 = require("ltn12")
-- the parameter may like below
-- base_url = "https://httpbin.org",
-- args = {
--  endpoint='/',
--  method='GET',
--  params={age=22,name='wer'},
--  -- a post header with content a=123
--  headers = {
--    ["Content-Type"] = "application/x-www-form-urlencoded",
--    ["Content-Length"] = 5
--  },
--  source = ltn12.source.string('a=123'),
--  step = {},
--  proxy = '',
--  redirect = true,
--  }

function http_request(base_url, args )
  local resp, r = {}, {}
  args.endpoint = args.endpoint or '/'
  args.method = args.method or 'GET'
  local params = ""
  if args.method == "GET" then
    -- prepare query parameters like q=23&a=2&
    if args.params then
      for i, v in pairs(args.params) do
          params = params .. i .. "=" .. v .. "&"
      end
    end
  end
  -- remove the last '&'
  params = string.sub(params, 1, -2)
  local url = base_url .. args.endpoint 
  if params then 
    url = url .. "?" .. params
  end
  local client, code, headers, status = http.request{
          url=url, 
          sink=ltn12.sink.table(resp),
          method=args.method,
          headers=args.headers, 
          source=args.source,
          step=args.step,
          proxy=args.proxy, 
          redirect=args.redirect, 
          create=args.create }
  r['code'], r['headers'], r['status'], r['response'] = code, headers, status, resp
  return r
end

http_request("http://192.168.2.5:8080", {endpoint='/ddd.zip'})
```




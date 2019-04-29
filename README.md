
# Apache2 and Nginx Cloudfront configuration

  

## Situation

  

Nginx or Apache2 are behind an AWS cloudfront distribution.

Nginx or Apache2 access logs show cloudfront distribution IP(s).

  

## Problems

- Unusable access statistics with tools like awstats;

- Fail2Ban triggers false positive, bans cloudfront IP(s);

- ... (others ?)

## Troubleshooting

Adding some new configuration directives to Apache2 or Nginx. With these,

the web servers are getting the real IP address from the `X-Forwarded-For`

HTTP header as it is set by cloudfront (see [doc](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html#RequestCustomIPAddresses)).

## Enabling

Apache2 ([remote ip module]([https://httpd.apache.org/docs/2.4/mod/mod_remoteip.html](https://httpd.apache.org/docs/2.4/mod/mod_remoteip.html)) has to be enabled) :

 - Copy the repository `/etc/apache2/conf-available/cloudfront.conf` file into your system `/etc/apache2/conf-available/` directory;
 - Enable the configuration file and restart apache2 :
	`a2enconf cloudfront`
	`systemctl restart apache2`
    
Nginx ([real ip module]([http://nginx.org/en/docs/http/ngx_http_realip_module.html](http://nginx.org/en/docs/http/ngx_http_realip_module.html)) has to be enabled) :

 - Copy the repository `/etc/nginx/conf.d/cloudfront.conf` file into your system `/etc/nginx/conf.d/` directory;
 - Restart nginx :
	`systemctl restart nginx`

Finally, check your server access logs.
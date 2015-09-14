---
title: Setting up a NodeJS server
date: 2015-09-14T15:04:59+02:00
layout: post
categories: git
---
For a project I needed to setup a webserver that was capable of capturing and logging all POST requests.

The idea was to capture all requests and store them all the filesystem.

I ended up writing this little script :


{% highlight javascript %}
var express = require('express');
var fs = require('fs');
var dateFormat = require('dateformat');
var bodyParser = require('body-parser')

var app = express();
app.use(bodyParser.json()); // for parsing application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded


// we can start the app by executing forever start server.js
//
// Sample CURL request : curl -H "Content-Type: application/json" -X POST -d '{"key":"xyz","value":"xyz"}' http://localhost:8181/rest/data

app.all("*",function(req, res, next) {
    console.log("req url  : " + req.url);
    console.log("req data : " + req.body);
    console.log("req data : " + JSON.stringify(req.body));

    var now = new Date();
	var nowAsString=dateFormat(now, "yyyy-mm-dd-hh-MM-ss");
	fileName = req.url.replace(/\//g, '-');
	fileName = nowAsString + fileName;

	fs.writeFile("/tmp/" + fileName, JSON.stringify(req.body), function(err) {
	    if(err) {
	        return console.log(err);
	    }
	}); 
	res.status(200).json({status:"ok"})
});

var server = app.listen(8181, function () {
  var host = server.address().address;
  var port = server.address().port;
  console.log('Server listening at http://%s:%s', host, port);
});
{% endhighlight %}


The script is started using ```forever```, a neat way to run your node program forever. More info on the [forever](https://github.com/foreverjs/forever) website.

# nessus-xmlrpc

Nessus XML RPC library and Nessus Command Line interface to XML RPC

(C) Vlatko Kosturjak, Kost. Distributed under MIT.

##Requirements
Requirements are quite standard Ruby libraries for HTTPS and XML
parsing:
```ruby
require 'uri'
require 'net/https'
require 'rexml/document'
```
##nessus-cli.rb
Nessus command line interface for XML-RPC.

```
Type ./nessus-cli.rb --help for command line options.
```
##Examples:
```
./nessus-cli.rb --user john --password doe --scan scan-localhost --wait --output report.xml --target localhost

./nessus-cli.rb --user user --password pass --scan localhost-scan --wait 5 -D --output report-localhost.xml --target localhost --verbose 

./nessus-cli.rb --user user --password pass --scan localhost-scan --wait 5 -D --output report-localhost.xml --target 127.0.0.1 --verbose --policy mypolicy --url https://localhost:8834
```
##Or if you want to have detached scans:
```
./nessus-cli.rb --user user --password pass --scan localhost-scan --target 127.0.0.1 --policy mypolicy

./nessus-cli.rb --user user --password pass --list-scans 

./nessus-cli.rb --user user --password pass --pause 5329fae9-fb1d-0c67-a401-a0db12637c0d5bcd67900d34e00e

./nessus-cli.rb --user user --password pass --resume 5329fae9-fb1d-0c67-a401-a0db12637c0d5bcd67900d34e00e

./nessus-cli.rb --user user --password pass --stop 5329fae9-fb1d-0c67-a401-a0db12637c0d5bcd67900d34e00e

./nessus-cli.rb --user user --password pass --stop-all

./nessus-cli.rb --user user --password pass --report 5329fae9-fb1d-0c67-a401-a0db12637c0d5bcd67900d34e00e --output report.xml
```
##nessus-xmlrpc.rb
communicate with Nessus(4.2+) over XML RPC interface

Simple example:
```ruby
require 'nessus-xmlrpc'
n=NessusXMLRPC::NessusXMLRPC.new('https://localhost:8834','user','pass');
# n=NessusXMLRPC::NessusXMLRPC.new('','user','pass'); # it's same
if n.logged_in
      id,name = n.policy_get_first
      puts "using policy ID: " + id + " with name: " + name
      uid=n.scan_new(id,"textxmlrpc","127.0.0.1")
      puts "status: " + n.scan_status(uid)
      while not n.scan_finished(uid)
	      sleep 10
      end
      content=n.report_file_download(uid)
      File.open('report.xml', 'w') {|f| f.write(content) }
end
```
Take a look at nessus-cli.rb for more advanced examples.

##= Contributing to nessus-xmlrpc
* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it
* Fork the project
* Start a feature/bugfix branch
* Commit and push until you are happy with your contribution
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Copyright
Copyright (c) 2010 Vlatko Kosturjak. See LICENSE.txt for
further details.


Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.provider "virtualbox" do |vm|
  	vm.memory=3072
  	vm.cpus=4
  end

  # run elasticsearch cluster
  config.vm.provision "docker" do |d|
    d.build_image "/vagrant/elasticsearch/2.4", args: "-t hhfactory/elasticsearch"
    d.build_image "/vagrant/kibana/4.6", args: "-t hhfactory/kibana"
    d.build_image "/vagrant/mysql/5.6", args: "-t hhfactory/mysql"
    d.build_image "/vagrant/logstash", args: "-t hhfactory/logstash"
    d.run "mysql", image: "hhfactory/mysql", args: "--name mysql -e MYSQL_ROOT_PASSWORD= -d -p 3306:3306"
    d.run "es0", image:"hhfactory/elasticsearch", args: "--name es0 -e NODE_NAME=node0 -p 9200:9200 -p 9300:9300 -d -it"
    d.run "es1", image:"hhfactory/elasticsearch", args: "--name es1 -e NODE_NAME=node1 -p 9201:9200 -p 9301:9300 -d -it"
    d.run "es2", image:"hhfactory/elasticsearch", args: "--name es2 -e NODE_NAME=node2 -p 9202:9200 -p 9302:9300 -d -it"
    d.run "hhfactory/kibana", args: "--name kibana -e ELASTICSEARCH_URL=http://192.168.33.10:9200 -p 5601:5601 -d"
    d.run "logstash", image: "hhfactory/logstash", args: "--name logstash -d"
  end
  
  config.vm.provision "shell", inline:
   "ps aux | grep 'sshd:' | awk '{print $2}' | xargs kill"

  config.vm.network :forwarded_port, guest:9200, host:9200
  config.vm.network :forwarded_port, guest:9201, host:9200
  config.vm.network :forwarded_port, guest:9202, host:9200
  config.vm.network :forwarded_port, guest:9300, host:9300
  config.vm.network :forwarded_port, guest:9301, host:9300
  config.vm.network :forwarded_port, guest:9302, host:9300
  config.vm.network :forwarded_port, guest:3306, host:3306
  config.vm.network :forwarded_port, guest:5601, host:5601
end

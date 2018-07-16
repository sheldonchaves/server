## Amazon ec2 + route53

##### Ec2
1 - Quick Launch Wizard
2 - Generate Key pair name

	chmod 400 keyName.pem
    
3 - Connect via SSH

	ssh -i "keyName.pem"((key name)) ubuntu@55.27.184.247 ((ip))
    
4 - Security Groups
	Quick Wizard to create a default FTP(22) ans HTTP(80)

##### Elastic IP
	One ip is free
    Allocate a new address and associete to instance ec252.27.184.24
    
##### Route 53 (DNS)

1 - Register a domain

2 - Enter Route 53

3 - Create Hosted Zone

4 - Create record set type A with WWW and value to IP

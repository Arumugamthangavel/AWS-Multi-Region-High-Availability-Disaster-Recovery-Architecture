
























* making sure httpd runnung on app-1
```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```  
* also, making sure apache2 running on app-2
```
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```  
* after that

* Mount F-files into Web Directory
```
sudo mount --bind /efs/F1 /var/www/html
```

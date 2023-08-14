# Installation

## **1. Ozon API & Event Monitor** 

1.  Clone the latest source code and put it on any directory you want. Here i assume you put it on /var

  
```
cd /var
git clone https://github.com/btechpt/ozon.git
cd ozon
```
2.  Create virtualenv (**Need activated**)

```
virtualenv env --python=python3.8
source env/bin/activate
pip install -r requirements.txt
```
3.  Update local_settings.py configuration file

```
cp ozon/local_settings.py.sample ozon/local_settings.py
vi ozon/local_settings.py
```

Bellow for configuration :
```
YUYU_NOTIFICATION_URL = "rabbit://openstack:password@127.0.0.1:5672//"
YUYU_NOTIFICATION_TOPICS = ["notifications"]

```

![ozon](assets/images/email_config.png)

You can get YUYU_NOTIFICATION_URL from neutron.conf, bellow if using kolla-ansible deployment.

```
cat /etc/kolla/neutron-server/neutron.conf | grep transport_url
```
4.  Run database migration

```
python manage.py migrate
```
5.  Ozon API installation
```
./bin/setup_api.sh
systemctl enable yuyu_api
systemctl start yuyu_api
systemctl status yuyu_api
```
6.  Ozon Event Monitor installation

```
./bin/setup_event_monitor.sh
systemctl enable yuyu_event_monitor
systemctl start yuyu_event_monitor
systemctl status yuyu_event_monitor
```
7.  Crontab installation

```
crontab -e
```

Add on the end file bellow :

```
1 0 1 * * /var/ozon/bin/process_invoice.sh
```
8. **Disable virtualenv**

Last step for Ozon API is need disabled virtualenv.

```
deactivate
```


## **2. Ozon Dashboard** 

1. Clone Repository

```
cd /var
git clone https://github.com/btechpt/yuyu_dashboard.git
cd yuyu_dashboard
```
2. Setup Ozon dashboard 


```
./setup_yuyu.sh

...
Enter horizon location and press ENTER.
example: /var/www/html/horizon
...
```
3. Install Ozon Dashboard Depencencies

```
pip3 install -r requirements.txt
```
4. Add config settings on horizon local_settings.py

```
vi /var/www/html/horizon/openstack_dashboard/local/local_settings.py
```
```
YUYU_URL="http://yuyu_server_url:8182"
CURRENCIES = ('IDR',)
DEFAULT_CURRENCY = "IDR"
```

For YUYU_URL you can use

```
YUYU_URL="http://127.0.0.1:8182"
```
5. Restart Horizon
```
systemctl restart apache2
```
6. Restart memcached (If dashboard login view not proper)
```
systemctl restart memcached
```

## **Verify Installation**

Access Dashboard, make sure Billing tab available in the navigation pane dashboard, next step need enabled billing & create pricing.
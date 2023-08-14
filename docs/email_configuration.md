# Email activating

Besides from notification center, we can receive billing invoice using email, for configuration 

- Update local_settings.py configuration file

```
cd /var/ozon/
nano ozon/local_settings.py
```

![ozon](assets/images/email_config.png)

- Running dataabase migration

```
python manage.py migrate
```

- After we configure email, now we received information from email

![ozon](assets/images/invoice_email.png)




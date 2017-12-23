# Afvalwijzer library

This library is meant to interface with http://www.mijnafvalwijzer.nl/

It is meant as a workaround for the afvalwijzer app (used in the Netherlands) to be notified when to place the bin at the road.
Since this app delivers a poor functionality for notifications, and I needed a small project, I created this.

### Installation
```bash
pip install afvalwijzer
```

### Usage
```python
>>> from Afvalwijzer import Afvalwijzer
>>> zipcode = '3564KV'
>>> number = '13'
>>> garbage = Afvalwijzer(zipcode, number)

>>> garbage.get_pickupdate()
datetime.datetime(2017, 12, 27, 0, 0)

>>> garbage.get_wastetype()
'Groente-, Fruit- en Tuinafval'

>>> garbage.afvaldatum()
(datetime.datetime(2017, 12, 27, 0, 0), 'Groente-, Fruit- en Tuinafval')
```

The following function returns true if the pickup date is the same as today.
```python
>>> garbage.notify()
```

Below is shown how I use it to get notified using pushbullet.
```python
#!/usr/bin/env python3

from Afvalwijzer import Afvalwijzer
from pushbullet import Pushbullet


def notification(device=None):
    pb = Pushbullet(pushbulletapi)
    try:
        mydevice = pb.get_device(device)
    except:
        mydevice = None
    push = pb.push_note(
        "Container: {}".format(wastetype),
        "Container: {}\nDate: {}".format(wastetype, pickupdate),
        device=mydevice)


zipcode = '3564KV'
number = '13'
pushbulletapi = 'pushbullet_api'
pushbulletdevice = 'LGE Nexus 5X'

garbage = Afvalwijzer(zipcode, number)
pickupdate, wastetype = garbage.garbage()
notify = garbage.notify()
if notify and pushbulletapi:
	notification(pushbulletdevice)
```

### Cron job
This script can now be set up as a cronjob on your server or alike.

```cron
0 6 * * * cd /path/to/script/notify_garbage.py > /dev/null 2>&1
```


## Contributors are most welcome
I'm still learning how to work with it all. Therefore feedback, advice, pull request etc. are most welcome.

# Scylla
> Spin up docker containers inline with python code for easier testing


## Install
```bash
git clone https://github.com/talosiot/scylla.git
make env
```

```
from IPython.core.display import display, HTML
import requests

import scylla
```

Run a docker container.  If there are ports that the container needs, specify them in a list.  If you want to wait until a certain port is available, you can specify that here as well.  For example, to run a small web server you need port 80 and you want to wait until it is available.

```
container = scylla.run_container(image='httpd:alpine', ports=[80], block_until_port_available=80)
container.ports
```




    {'80/tcp': [{'HostIp': '0.0.0.0', 'HostPort': '8228'}]}



The container is running and you can interact with it.

```
host_d = scylla.lookup_host(container, container_port=80)
url = 'http://{HostIp}:{HostPort}'.format(**host_d)
resp = requests.get(url)
print(resp.text)
display(HTML(resp.text))
```

    <html><body><h1>It works!</h1></body></html>
    



<html><body><h1>It works!</h1></body></html>



Stop the container any time.  Or, it will automatically stop at the end of the program.

```
container.stop()
```

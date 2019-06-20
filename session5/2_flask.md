## Flask

Flask is a Python package that allows you to build simple web apps.

```bash
mkdir flask-app-tutorial
cd flask-app-tutorial
```

If you have Python 2:
```bash
python -m virtualenv env
source env/bin/activate
pip install Flask requests
```

Python 3:
```bash
python3 -m venv env
source env/bin/activate
pip install Flask requests
```

This might be useful if you're on the VF network:
```bash
export http_proxy=http://vfukukproxy.internal.vodafone.com:8080
export https_proxy=http://vfukukproxy.internal.vodafone.com:8080
```

Create a new file called `app.py` and enter the following code:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return 'Hello, this is my website'

if __name__ == '__main__':
    app.run()
```

The in the command line run: `python app.py`

Switch over to a browser and go to http://localhost:5000/.

In another terminal, `cd` to the same directory and activate the virtual environment as above.

Go into a Python prompt by running `python`.

```bash
>>> import requests
>>> response = requests.get('http://localhost:5000/')
>>> response
>>> response.text
```

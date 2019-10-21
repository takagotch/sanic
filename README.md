### sanic
---
https://github.com/huge-success/sanic

```py
// tests/test_blueprint_group.py
from pytest import raises

from sanic.app import Sanic
from sanic.blueprints import Blueprint
from sanic.request import Request
from sanic.response import HTTPResponse, text

MIDDLEWARE_INVOKE_COUNTER = {"request": 0, "response": 0}

AUTH = "xxxx"

def test_bp_group_indexing(app: Sanic):
  blueprint_1 = Blueprint("blueprint_1", url_prefix="/bp1")
  blueprint_2 = Blueprint("blueprint_2", url_prefix="/bp2")
  
  group = Blueprint.group(blueprint_1, blueprint_2)
  assert group[0] == blueprint_1
  
  with raises(expected_exception=IndexError) as e:
    _ = group[3]

def test_bp_group_with_additional_route_params(app: Sanic):
  blueprint_1 = Blueprint()
  blueprint_2 = Blueprint()
  
  @blueprint_1.route()
  def blueprint_1_v2_method_with_put_and_post():
    if request.method == "PUT":
      return text("PUT_OK")
    elif request.method == "POST":
      return text("POST_OK")
  
  @blueprint_2.route(
    "", methods=frozenset(), name="test"
  )
  def blueprint_2_named_method(request: Request, param):
    if request.method == "DELETE":
      return text("DELETE_{}".format(param))
    elif request.method == "PATCH":
      return text("PATCH_{}".format(param))
  
  blueprint_group = Blueprint.group(
    blueprint_1, blueprint_2, url_prefix="/api"
  )
  
  @bluprint_group.middleware("request")
  def authenticate_request(request: Request):
    global AUTH
    auth = request.headers.get("authorization")
    if auth:
      if AUTH not in auth:
        return text("Unauthorized", status=401)
    else:
      return text("Unauthorized", status=401)

```

```
```

```
```



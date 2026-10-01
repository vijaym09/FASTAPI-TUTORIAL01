# FastAPI Day 2 — Path Parameters, Validation & Enums

Today I learned about **Path Parameters**, **Type Hints**, **HTTPException**, **Status Codes**, and **Enums** in FastAPI.

I also learned how FastAPI automatically validates path parameters and shows the API information in Swagger UI.

---

## Topics I Learned

* Path Parameters
* Type Hints
* `HTTPException`
* HTTP Status Codes
* Swagger UI (`/docs`)
* Data Validation
* Python `Enum`
* Using Enum with Path Parameters

---

# 1. FastAPI App Setup

I started with the basic FastAPI application.

```python
from fastapi import FastAPI

app = FastAPI()
```

Here:

* `FastAPI()` creates the FastAPI application.
* `app` is used with decorators like `@app.get()`.
* The decorator connects a URL path with a Python function.

Example:

```python
@app.get("/")
def home():
    return {"message": "Hello World"}
```

---

# 2. Creating a List Endpoint

First, I created a list of bands using Python dictionaries.

```python
bands = [
    {"id": 1, "name": "The Kinks", "genre": "rock"},
    {"id": 2, "name": "Aphex Twin", "genre": "electronic"},
    {"id": 3, "name": "Black Sabbath", "genre": "metal"},
    {"id": 4, "name": "Something Else", "genre": "hip-hop"},
]
```

Then I created an endpoint to return all bands.

```python
@app.get("/bands")
def bands() -> list[dict]:
    return bands
```

When I open:

```text
http://localhost:8000/bands
```

I get the complete list of bands in JSON format.

### Important

`/bands` represents the collection of bands.

So generally:

```text
/bands
```

means all bands.

And later:

```text
/bands/1
```

can represent one particular band.

---

# 3. Running the FastAPI Server

I can run the application using:

```bash
uvicorn main:app --reload
```

`--reload` means the server automatically restarts when I make changes in the code.

If I open:

```text
http://localhost:8000/
```

and I haven't created a `/` route, FastAPI returns:

```text
Not Found
```

So I need to open the endpoint I actually created:

```text
http://localhost:8000/bands
```

---

# 4. Path Parameters

Now I wanted to get only one band using its ID.

For this, I can use a **path parameter**.

Example:

```text
/bands/{band_id}
```

The `{band_id}` part is the variable part of the URL.

Example:

```text
/bands/1
/bands/2
/bands/3
```

Here, `1`, `2`, and `3` are different values of `band_id`.

In FastAPI:

```python
@app.get("/bands/{band_id}")
def band(band_id: int) -> dict:
    ...
```

The important thing here is:

```python
band_id: int
```

I am telling FastAPI that `band_id` should be an integer.

---

# 5. Finding a Band by ID

I used Python's `next()` function to find the band.

```python
@app.get("/bands/{band_id}")
def band(band_id: int) -> dict:
    band = next((b for b in bands if b["id"] == band_id), None)

    if band is None:
        raise HTTPException(status_code=404, detail="Band not found")

    return band
```

### How this works

This part:

```python
b for b in bands if b["id"] == band_id
```

checks the bands one by one and looks for a matching ID.

The `None` at the end means that if no band is found, `next()` returns `None`.

Then:

```python
if band is None:
```

checks whether the band exists.

If it doesn't exist, I raise a `404` error.

---

# 6. HTTPException

For returning an HTTP error, FastAPI provides `HTTPException`.

I need to import it:

```python
from fastapi import FastAPI, HTTPException
```

Then I can use:

```python
raise HTTPException(
    status_code=404,
    detail="Band not found"
)
```

Here:

* `404` means **Not Found**
* `detail` contains the message returned to the client.

For example, if I request:

```text
/bands/100
```

and there is no band with ID `100`, the API returns a `404` error.

---

# 7. One Mistake I Faced

One important thing I learned was related to the **return type**.

If I write:

```python
def band(band_id: int) -> str:
```

but actually return a dictionary:

```python
return band
```

it can cause an error because the function says that it will return a `str`, but I am returning a dictionary.

So I should use:

```python
def band(band_id: int) -> dict:
```

This is one reason why type hints are important.

---

# 8. FastAPI Automatic Validation

Because I wrote:

```python
band_id: int
```

FastAPI automatically checks whether the value can be converted to an integer.

For example:

```text
/bands/1
```

works.

But:

```text
/bands/abc
```

doesn't work because `abc` is not an integer.

FastAPI returns:

```text
422 Unprocessable Entity
```

So I don't have to manually check wh

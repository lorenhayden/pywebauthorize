# PyWebAuthorization
The PyWebAuthorize library is intended to simplify all 
web based authorization

## Install PyWebAuthorization
```bash
pip install pywebauthorization
```

## PyWebAuthorization Hierarchy
- **basicauth**
  - **encode**
    - **parameters**
      - **user** *(str)* The username of the user
      - **password** *(str)* The password of the user
      - **use_hashed_password** *(bool)* Flag to indicate password is hashed using SHA3-512 algorithm
    - **return** *(str)* Basic Authorization string ```Basic <Base64 encoded string>```
    - **raises** *(BasicAuthError)* Raises exception with reason
  - **decode**
    - **parameters**
      - **authorization** *(str)* The basic authorization string ```Basic <Base64 encoded string>```
    - **return** *(dict)* Returns the user and password in a key value pair dictionary
    - **raises** *(BasicAuthError)* Raises exception with reason
  - **verify**
    - parameters
      - **authorization** *(str)* The basic authorization string
      - **use_hashed_password** *(bool)* Flag to indicate password is hashed using SHA3-512 algorithm
    - **return** *(bool)* True = Authorized, False = Not Authorized
    - **raises** *(BasicAuthError)* Raises exception with reason

## Documentation

### basicauth.encode
Encodes a username and password into a valid Basic Authorization string

#### Example

```python
from pywebauthorization import basicauth, BasicAuthError

try:
  # Note: 
  # If the use_hashed_password is False:
  # Basic dGVzdDoxMjM0NQ==
  # If the use_hashed_password is True: 
  # Basic dGVzdDowYTJhMTcxOWJmM2NlNjgyYWZkYmVkZjNiMjM4NTc4MThkNTI2ZWZiZTdmY2IzNzJiMzEzNDdjMjYyMzlhMGY5MTZjMzk4YjdhZDhkZDBlZTc2ZThlMzg4NjA0ZDBiMGY5MjVkNWU5MTNhZDJkMzE2NWI5YjM1YjM4NDRjZDVlNg== 
  user_name = 'test'
  user_password = '12345'
  basic_auth = basicauth.encode(user=user_name, password=user_password)
  print(f'{basic_auth}')
except BasicAuthError as e:
  print(f'{e}')
```
### basicauth.decode
Decode the Basic Authorization string into a dictionary
```python
# Example output
{
  'user': 'test', 
  'password': '12345'
}
```
### Code Example

```python
from pywebauthorization import basicauth, BasicAuthError

try:
  # Note: 
  # This should print out dictionary
  # {'user': 'test', 'password': '12345'}
  basic_auth_plain_text = 'Basic dGVzdDoxMjM0NQ=='
  basic_auth = basicauth.decode(authorization=basic_auth_plain_text)
  print(f'{basic_auth}')
except BasicAuthError as e:
  print(f'{e}')
```
### basicauth.verify
Verify a basic authorization string sent from client with username and password  
### Code Example

```python
from pywebauthorization import basicauth, BasicAuthError

try:
  # Note: 
  # This should print 'Verified: True'
  basic_auth_plain_text = 'Basic dGVzdDoxMjM0NQ=='
  user_auth = {'user': 'test', 'password': '12345'}
  verified = basicauth.verify(authorization=basic_auth_plain_text, user=user_auth)
  print(f'Verified: {verified}')
except BasicAuthError as e:
  print(f'{e}')
```
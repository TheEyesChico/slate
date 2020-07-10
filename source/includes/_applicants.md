# Applicants

## About applicants

An applicant represents an individual who will be the subject of [a check](http://cw.sparshik.com:8006/#kittens). An applicant must exist before a check can be initiated.

## Applicant object 

> An example applicant object

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "id": "<APPLICANT_ID>",
  "created_at": "2019-10-09T16:52:42Z",
  "sandbox": true,
  "first_name": "Jane",
  "last_name": "Doe",
  "email": null,
  "dob": "1990-01-01",
  "delete_at": null,
  "href": "/v3/applicants/<APPLICANT_ID>",
  "id_numbers": [],
  "address": {
    "flat_number": null,
    "building_number": null,
    "building_name": null,
    "street": "Second Street",
    "sub_street": null,
    "town": "London",
    "state": null,
    "postcode": "S2 2DF",
    "country": "GBR",
    "line1": null,
    "line2": null,
    "line3": null
  }
}
```

**Attribute** | **Description**
------------- | ---------------
id            |    **string** <br> The unique identifier for the applicant
created_at    |    **datetime** <br> The date and time when this applicant was created
delete_at    |    **datetime** <br> The date and time when this applicant is scheduled to be deleted, or `null` if the applicant is not scheduled to be deleted
href    |    **string** <br> The URI of this resource
first_name    |    **string** <br> The applicant’s first name
last_name   |    **string** <br> The applicant’s surname
email   |    **string** <br> The applicant’s email address
dob   |    **date** <br> The applicant’s date of birth in yyyy-mm-dd format
id_numbers   |    **array of id number objects** <br> A collection of identification numbers belonging to this applicant
address   |    **[address](http://cw.sparshik.com:8006/#address-object) object** <br> The address of the applicant


### ID NUMBER

**Attribute** | **Description**
------------- | ---------------
type          |    **string** <br> Type of ID number. Valid values are `ssn`, `social_insurance` (e.g. UK National Insurance), `tax_id`, `identity_card` and `driving_licence`
value         |    **string** <br> Value of ID number <br> Note that `ssn` supports both the full SSN or the last 4 digits. If providing the full SSN the value has to be supplied with dashes in the following format: xxx-xx-xxxx
state_code    |    **string** <br> Two letter code of issuing state (state-issued driving licences only) 

<aside class="notice">
The API uses the spelling “driving_licence”
</aside>

## Address object
**Attribute** | **Description**
------------- | ---------------
flat_number            |    **string** <br> The flat number
building_number            |    **string** <br> The building number
building_name           |    **string** <br> The building name
street            |    **string** <br> The street of the applicant’s address. There is a 32-character limit on this field for UK addresses
sub_street            |    **string** <br> The sub-street
town            |    **string** <br> The town
state            |    **string** <br> The address state. US states must use the USPS abbreviation (see also [ISO 3166-2:US](https://www.iso.org/obp/ui/#iso:code:3166:US)), for example `AK`, `CA`, or `TX`.
postcode            |    **string** <br> The postcode or ZIP of the applicant’s address. For UK postcodes, specify the value in the following format: `SW4` `6EH`
country           |    **string** <br> The 3 character ISO country code of this address. For example, `GBR` is the country code for the United Kingdom
line1            |    **string** <br> Line 1 of the address
line2           |    **string** <br> Line 2 of the address
line3           |    **string** <br> Line 3 of the address

The applicant address object is nested inside the address object. <br><br>
`postcode` and `country` are required fields if an address is provided for an applicant. For US addresses, `state` is also a required field.<br><br>
Most addresses will contain other information such as `flat_number`. Please make sure they are supplied as separate fields, and do not try and fit them all into the `street` field. Doing so is likely to affect check performance. Alternatively, you can provide addresses in the form `line1`, `line2` and `line3`.<br>

## Forbidden characters

For addresses the following characters are forbidden:

`!$%^*=<>`

For names the following characters are forbidden:

`^!#$%*=<>;{}"`

## Create applicant

> Create an applicant

```http
POST /v3/applicants/ HTTP/1.1
Host: api.sparshik.com
Authorization: Token token=<YOUR_API_TOKEN>
Content-Type: application/json
```
```shell
$ curl https://api.onfido.com/v3/applicants/ \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
  "first_name": "Jane",
  "last_name": "Doe",
  "dob": "1990-01-31",
  "address": {
    "building_number": "100",
    "street": "Main Street",
    "town": "London",
    "postcode": "SW4 6EH",
    "country": "GBR"
  }
}'
```

```ruby
applicant_details = {
  first_name: "Jane",
  last_name: "Doe",
  dob: "1990-01-01",
  address: {
    building_number: "100",
    street: "Second Street",
    town: "London",
    postcode: "S2 2DF",
    country: "GBR"
  }
}

onfido.applicant.create(applicant_details)
```

```python
applicant_address = sparshik.Address(
    building_number=100,
    street='Main Street',
    town='London',
    postcode='SW4 6EH',
    country='GBR'
)

applicant_details = sparshik.Applicant(
    first_name='Jane',
    last_name='Doe',
    address=applicant_address
)

api_instance.create_applicant(applicant_details)
```

```php
$applicantDetails = new Onfido\Model\Applicant();

$applicantDetails->setFirstName('Jane');
$applicantDetails->setLastName('Doe');
$applicantDetails->setDob('1990-01-31');

$address = new \Onfido\Model\Address();
$address->setBuildingNumber('100');
$address->setStreet('Main Street');
$address->setTown('London');
$address->setPostcode('SW4 6EH');
$address->setCountry('GBR');

$applicantDetails->setAddress($address);

$onfido->createApplicant($applicantDetails);
```

```java
import com.onfido.models.Address;
import com.onfido.models.Applicant;

Address.Request addressRequest = Address.request()
        .street("Main Street")
        .town("London")
        .postcode("SW4 6EH")
        .country("GBR");

Applicant.Request applicantRequest = Applicant.request()
        .firstName("Jane")
        .lastName("Doe")
        .dob(LocalDate.parse("1990-01-31"))
        .address(addressRequest);

Applicant applicant = onfido.applicant.create(applicantRequest);
```

```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "dob": "1990-01-31",
  "address": {
    "building_number": "100",
    "street": "Main Street",
    "town": "London",
    "postcode": "SW4 6EH",
    "country": "GBR"
  }
}
```
<span class="post">POST</span><span class="url">https://api.onfido.com/v3/applicants/</span>

<aside class="notice">
Using this endpoint in a live context will cause you to send personal data to Onfido. Always make sure you inform your users about this and obtain any necessary permissions. For more information on how Onfido uses personal data, view our <a href="">Privacy Policy</a>
</aside>

Creates a single [applicant.](http://cw.sparshik.com:8006/#about-applicants) Returns an [applicant object](http://cw.sparshik.com:8006/#applicant-object).
<br><br>
When you create an applicant, [some characters are forbidden](http://cw.sparshik.com:8006/#forbidden-characters).
<br><br>
Required applicant data differs depending on report type. For example, for [Document reports]().
<br>
### REQUEST BODY PARAMETERS
|                   |               |
|-------------------|---------------|
| first_name      | **required** <br> The applicant’s forename.
| last_name             | **required** <br> The applicant’s surname.
| email             |  **required only if creating a check where**<br> `applicant_provides_data` is `true` <br> The applicant’s email address.
|dob           |  **optional**<br> The applicant’s date of birth in yyyy-mm-dd format.
| id_numbers	   |  **optional**<br> A collection of identification numbers belonging to this applicant
|address   |  **optional**<br> The address of the applicant

## Retrieve applicant

> Retrieve an applicant

```http
GET /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.onfido.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl https://api.onfido.com/v3/applicants/<APPLICANT_ID> \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>'
```

```ruby
onfido.applicant.find('<APPLICANT_ID>')
```

```python
api_instance.find_applicant('<APPLICANT_ID>')
```

```php
$onfido->findApplicant('<APPLICANT_ID>');
```

```java
import com.onfido.models.Applicant;

String applicantId = "<APPLICANT_ID>";

Applicant applicant = onfido.applicant.find(applicantId);
```

<span class="get">GET</span><span class="url">https://api.onfido.com/v3/applicants/{applicant_id}</span>
<br><br>
Retrieves a single [applicant](http://cw.sparshik.com:8006/#about-applicants). Returns an [applicant object](http://cw.sparshik.com:8006/#applicant-object).

## Update applicant

> Update an applicant

```http
PUT /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.onfido.com
Authorization: Token token=<YOUR_API_TOKEN>
Content-Type: application/json
```

```shell
$ curl https://api.onfido.com/v3/applicants/<APPLICANT_ID> \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
  "first_name": "New",
  "last_name": "Name"
}' \
  -X PUT
```

```ruby
new_applicant_details = {
  first_name: "New",
  last_name: "Name"
}

onfido.applicant.update('<APPLICANT_ID>', new_applicant_details)
```

```python
new_applicant_details = onfido.Applicant()

new_applicant_details.first_name = 'New'
new_applicant_details.last_name = 'Name'

api_instance.update_applicant('<APPLICANT_ID>', new_applicant_details)
```

```php
$newApplicantDetails = new Onfido\Model\Applicant();
$newApplicantDetails->setFirstName('New');

$onfido->updateApplicant('<APPLICANT_ID>', $newApplicantDetails);
```

```java
import com.onfido.models.Applicant;

Applicant.Request newApplicantDetails = Applicant.request()
        .firstName("New")
        .lastName("Name");

String applicantId = "<APPLICANT_ID>";

Applicant applicant = onfido.applicant.update(applicantId, newApplicantDetails);
```

```json
{
  "first_name": "New",
  "last_name": "Name"
}
```

<span class="put">PUT</span><span class="url">https://api.onfido.com/v3/applicants/{applicant_id}</span>
<br>
<aside class="notice">
Using this endpoint in a live context will cause you to send personal data to Onfido. Always make sure you inform your users about this and obtain any necessary permissions. For more information on how Onfido uses personal data, view our [Privacy Policy]().
</aside>

Updates an applicant’s information. Returns the updated [applicant object](http://cw.sparshik.com:8006/#applicant-object).
<br>

* Partial updates
* Addresses and ID numbers present will replace existing ones
* Same applicant validations to create applicant

### REQUEST BODY PARAMETERS
|                   |               |
|-------------------|---------------|
| first_name      | The applicant’s forename.
| last_name             | The applicant’s surname.
| email             |  The applicant’s email address.
|dob           |  The applicant’s date of birth in yyyy-mm-dd format.
| id_numbers	   |  A collection of identification numbers belonging to this applicant
|address   |  The address of the applicant

## Delete applicant

> Delete an applicant

```http
DELETE /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.onfido.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl https://api.onfido.com/v3/applicants/<APPLICANT_ID> \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' \
  -X DELETE
```

```ruby
onfido.applicant.destroy('<APPLICANT_ID>')
```

```python
api_instance.destroy_applicant('<APPLICANT_ID>')
```

```php
$onfido->destroyApplicant('<APPLICANT_ID>');
```

```java
import com.onfido.models.Applicant;

String applicantId = "<APPLICANT_ID>";

onfido.applicant.delete(applicantId);
```

<span class="delete">DELETE</span><span class="url">https://api.onfido.com/v3/applicants/{applicant_id}</span>
<br><br>
Deletes a single applicant. If successful, returns a `204 No Content` response. 
<br><br>
Sending a deletion request adds the applicant object and all associated documents, photos, videos, checks, reports and analytics data to our deletion queue. The objects will be permanently deleted from Onfido’s production object storage and relational database system, after a deletion delay which can be configured by [emailing our client support team](). After deletion, applicant details cannot be recovered or queried, and Onfido will not be able to troubleshoot. Within the delay period, the applicant can be restored. For more information about Onfido’s deletion service, see our [Data Deletion FAQ]().

<aside class="notice">
Deleting an applicant that does not have an associated check will immediately delete the applicant and any resources.
</aside>

## Restore applicant

> Restore an applicant

```http
POST /v3/applicants/<APPLICANT_ID>/restore HTTP/1.1
Host: api.onfido.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl https://api.onfido.com/v3/applicants/<APPLICANT_ID>/restore \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' \
  -X POST
```

```ruby
onfido.applicant.restore('<APPLICANT_ID>')
```

```python
api_instance.restore_applicant('<APPLICANT_ID>')
```

```php
$onfido->restoreApplicant('<APPLICANT_ID>');
```

```java
import com.onfido.models.Applicant;

String applicantId = "<APPLICANT_ID>";

onfido.applicant.restore(applicantId);
```
<span class="post">POST</span><span class="url">https://api.onfido.com/v3/applicants/{applicant_id}/restore</span>
<br><br>
Restores a single applicant scheduled for deletion. If successful, returns a `204 No Content` response.
<br><br>
A restore request will also restore all associated documents, photos, videos, checks, reports and analytics data.
<br><br>
Applicants that have been permanently deleted cannot be restored.

##  List applicants

> Return all applicants you’ve created

```http
GET /v3/applicants/ HTTP/1.1
Host: api.onfido.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl https://api.onfido.com/v3/applicants/ \
  -H 'Authorization: Token token=<YOUR_API_TOKEN>'
```

```ruby
onfido.applicant.all(page: 1, per_page: 20)
```

```python
api_instance.list_applicants(page=1, per_page=20, include_deleted=True)
```

```php
$onfido->listApplicants();
```

```java
import com.onfido.models.Applicant;

Integer page = 1;
Integer perPage = 20;
Boolean includeDeleted = false; 

List<Applicant> applicants = onfido.applicant.list(page, perPage, includeDeleted);
```
<span class="get">GET</span><span class="url">https://api.onfido.com/v3/applicants/</span>
<br><br>
Lists all applicants you’ve created, sorted by creation date in descending order.
<br><br>
Returns data in the form: <br> `{"applicants": [<LIST_OF_APPLICANT_OBJECTS>]}.`
<br><br>
Requests to this endpoint will be paginated to 20 items by default.

### QUERY STRING PARAMETERS

`include_deleted=true` (optional): include applicants scheduled for deletion.
<br><br>
`per_page` (optional): set the number of results per page. Defaults to 20.
<br><br>
`page` (optional): return specific pages. Defaults to 1.

### LINK HEADER

The `Link` header contains pagination information.<br>
For example:
<br><br>
`Link: <https://api.onfido.com/v3/applicants?page=3059>; rel="last", <https://api.onfido.com/v3/applicants?page=2>; rel="next"`
<br><br>
Possible `rel` values are:
<br>

| **Name** |**Link relation (description)**
|----------|---------------------------------
|`next`    |    Next page of results
|`last`    |    Last page of results
|`first`   |    First page of results
|`prev`    |    Previous page of results
<br>
The custom `X-Total-Count` header gives the total resource count.
<br><br>
You can read more about `Link` headers [on the IETF website](https://tools.ietf.org/html/rfc5988).

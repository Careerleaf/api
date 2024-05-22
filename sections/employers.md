# Employers data

Use this API to migrate your existing employers (company info and users) to the Careerleaf platform




## Employer Endpoints

* `GET /app/api/v1/employers` -> paginated list of employers
* `GET /app/api/v1/employers/$id/` - single employer
* `POST /app/api/v1/employers/` - create new 
* `PUT /app/api/v1/employers/$id` - update existing (overwrite)
* `PATCH /app/api/v1/employers/$id` - update existing (partial update)
* `GET /app/api/v1/employers/quick-list/` - paginated list of employers with only few fields returned (id, old_id etc..)




## Create new employer

    POST /app/api/v1/employers


#### Request BODY
Below sample data will create employer 'ABC INC' with 3 companies 'ABC INC', 'ABC Consultancy' and 'ABC Exports' and 2 users. 
By default 'ABC INC' will be the default company with description "ABC Inc description" . However, the flag 'is_default=1' will overwrite and make 'ABC Consultancy' as default company.

Note: the following is not a valid *JSON* document, given that it has comments. Your actual request shouldn't have them. Take a look at the sample GET response below for an example of valid JSON.

```js

{
      "name": "ABC Inc", //required
      "site_url": "https://abc.com",
      "old_id": "abc123", //required
      "desc": "ABC Inc description",
      "billing_address":{
        "city": "Miami",
        "fax": "+1 323 555 1234",
        "country": "United States", //required
        "street2": "",
        "phone": "+1-541-754-3010",
        "state": "Florida",
        "street": "Street",
        "postal_code": "32024",
        "country_code": "US"
      },
      "users": [
        {
          "first_name": "Admin", //required
          "last_name": "V",
          "email": "admin@abc.com", //required
          "access_group": "admin"
        },
      {
          "first_name": "Kristin", //required
          "last_name": "may", 
          "email": "km@example.com", //required
          "access_group": "LA" 
        }],
        "companies":[
        {
          "name": "ABC Consultancy", //required
          "url": "https://abcconsultancy.com",
          "desc": "Description for ABC Consultancy", //company description
          "is_default": 1, //Default company
          "logo_url": "https://i2.wp.com/storage.whmcs.community/monthly_2017_09/T.png.f6c2896bc17da48204db15019ed38915.png"
        },
         {
          "name": "ABC Exports", //required
          "url": "https://abcexports.com",
          "desc": "Description for ABC Exports", //company description
          "is_default": 0
        }
        ],
        "inviter":{
                "email": "davidsmith@example.com",
                "first_name": "David",
                "last_name": "Smith"
                },
        "is_ats_enabled": true,
        "is_active": true
}
```


## List of employers

    GET /app/api/v1/employers

### Parameters 
    page_kwarg = 'page'
    paginate_by_param = 'page_size' (the maximum page_size is restricted to 100)

### Response 

``` json 
{
  "count": 5,
  "next": null,
  "previous": null,
  "results": [
  {
   "name": "ABC Inc",
   "old_id": "abc123",
   "site_url": "https://abc.com",
   "created_on": "2019-08-28T05:26:03.848",
   "def_company": "ABC Consultancy",
   "id": "5d66106b0640fd7805f807e2",
   "is_ats_enabled": false,
   "is_active": true,
   "billing_address": {
      "city": "Miami",
      "country": "United States",
      "country_code": "US",
      "fax": "+1 323 555 1234",
      "phone": "+1-541-754-3010",
      "postal_code": "32024",
      "state": "Florida",
      "state_id": 54,
      "street": "Street",
      "street2": ""
    },
   "companies": [
      	{"desc": "ABC Inc description", "name": "ABC Inc", "url": ""},
      	{"desc": "Description for ABC Exports","name": "ABC Exports","url": "https://abcexports.com"},
      	{"desc": "Description for ABC Consultancy", "name": "ABC Consultancy","url": "https://abcconsultancy.com"},
  	  ],
   "users": [
      	{"first_name": "Admin", "id": "5d66106c0640fd7805f807e6", "last_name": "V","access_group": "admin", "email": "admin@abc.com"},
      	{"first_name": "Kristin", "id": "5d66106c0640fd7805f807e7","last_name": "may","access_group": "LA", "email": "km@example.com"}
      ]
  },
,
{
   "name": "XYZ Inc",
   "old_id": "xyz123",
   "site_url": "https://xyz.com",
   "created_on": "2018-08-28T05:26:03.848",
   "def_company": "XYZ Consultancy",
   "id": "5d63456b0640fd7805f807e2",
   "is_ats_enabled": false,
   "is_active": true
   "billing_address": {
      "city": "Toronto",
      "country": "Canada",
      "country_code": "CA",
      "fax": "+1 323 555 3456",
      "phone": "+1-555-555-5555",
      "postal_code": "M4B 1B3",
      "state": "Ontario",
      "state_id": 54,
      "street": "Street",
      "street2": ""
    },
   "companies": [
        {"desc": "", "name": "XYZ Inc", "url": ""},
        {"desc": "Description for XYZ Exports","name": "XYZ Exports","url": "https://xyzexports.com"},
        {"desc": "Description for XYZ Consultancy", "name": "XYZ Consultancy","url": "https://xyzconsultancy.com"},
      ],
   "users": [
        {"first_name": "Admin", "id": "5d66106c0646665f807e6", "last_name": "V","access_group": "admin", "email": "root@xyz.com"},
        {"first_name": "Kris", "id": "5d66106c06666d7805f807e7","last_name": "may","access_group": "LA", "email": "krisexample.com"}
      ]
  }
  ]
}

```

## Quck list of employers

use it to quickly iterate through existing records, that can be useful during migration re-run

    GET /app/api/v1/employers/quick-list/


### Parameters 

    page_kwarg = 'page'
    paginate_by_param = 'page_size' (the maximum page_size is restricted to 500)

### Response 

```json 

{
  "count": 1310,
  "next": "http://docker:8008/app/api/v1/employers/quick-list?page=2&page_size=3",
  "previous": null,
  "results": [
    {
      "id": "54be99e3ae36ba1a6006efe2",
      "old_id": "1",
      "name": "T Hosp",
      "created_on": "2015-01-20T18:09:39.791"
    },
    {
      "id": "54bf3d47ae36ba1a6006efe8",
      "old_id": "2",
      "name": "A Comp",
      "created_on": "2015-01-21T05:46:47.150"
    },
    {
      "id": "54c02299ae36ba556c06efef",
      "old_id": null,
      "name": "ABC Medical Group",
      "created_on": "2015-01-21T22:05:13.941"
    }
  ]
}

```

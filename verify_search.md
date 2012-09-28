# Redsson Parachute API: Verify #


Using the Parachute Verify API, you can confirm customer elements such as name, SSN, address, DOB, phone.

## Request ##

### HTTP POST to /verify/search ###

    https://parachute.redsson.com/verify/search

### POST Parameters ###

**Required Parameters**

You must POST the following parameters:

<table>
  <tr>
    <th>Parameter</th><th>Description</th>
  </tr>
  <tr>
    <td>data</td><td>This parameter will contain the XML data for the request</td>
  </tr>
</table>

XML Input Fields:

<table>
  <tr>
    <th>Field</th><th>Description</th><th>Notes</th>
  </tr>
  <tr>
    <td>firstName</td><td>First Name</td><td>Required</td>
  </tr>
  <tr>
    <td>lastName</td><td>Last Name</td><td>Required</td>
  </tr>
  <tr>
    <td>sSN</td><td>Social Security Number</td><td>Must be supplied if Address information and Date of Birth is blank (format 9 digits)</td>
  </tr>
  <tr>
    <td>dOB</td><td>Date of Birth</td><td>Must be supplied if Address information and SSN is blank (format DD/MM/YYYY)</td>
  </tr>
  <tr>
    <td>streetAddress</td><td>Street Address</td><td>Must be supplied if Date of Birth and SSN is blank</td>
  </tr>
  <tr>
    <td>city</td><td>City</td><td>Must be supplied if Date of Birth and SSN is blank</td>
  </tr>
  <tr>
    <td>state</td><td>State</td><td>Must be supplied if Date of Birth and SSN is blank</td>
  </tr>
  <tr>
    <td>zip</td><td>Zip</td><td>Optional</td>
  </tr>
  <tr>
    <td>homePhone</td><td>Home Phone</td><td>Optional (format 10 digits)</td>
  </tr>
</table>

Example:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<request>
  <data>
    <firstName>Test</firstName>
    <lastName>User</lastName>
    <sSN>123456789</sSN>
    <dOB>03/31/1977</dOB>
    <streetAddress>104 N. Summit St. FL 2</streetAddress>
    <city>Toledo</city>
    <state>OS</state>
    <zip>43604</zip>
    <homePhone>4192441111</homePhone>
  </data>
</request>
```


**Optional Parameters**

You may POST the following parameters:

<table>
  <tr>
    <th>Parameter</th><th>Description</th>
  </tr>
  <tr>
    <td>user</td><td>This parameter will contain the end user name used for report tracking</td>
  </tr>
</table>

### Authentication ###

**Basic Authentication**

The Redsson Verify API uses HTTP Basic Authentication. You will receive an API username and password for this service.

When the user agent wants to send the server authentication credentials it may use the Authorization header

    The Authorization header is constructed as follows:

    1. Username and password are combined into a string "username:password"
    2. The resulting string literal is then encoded using Base64
    3. The authorization method and a space (i.e. "Basic ") is then put before 
       the encoded string.

For example, if the user agent uses 'Aladin' as the username and 'sesam open' as the password then the header is formed as follows:
    
    Authorization: Basic QWxhZGluOnNlc2FtIG9wZW4=


## Response ##

XML Response Fields

Validated Fields each contain the result of the search as well as the status (verified, not_verified, corrected, missing)

<table>
  <tr>
    <th>Field</th><th>Description</th>
  </tr>
  <tr>
    <td>firstName</td><td>First Name</td>
  </tr>
  <tr>
    <td>lastName</td><td>Last Name</td>
  </tr>
  <tr>
    <td>sSN</td><td>Social Security Number</td>
  </tr>
  <tr>
    <td>dOB</td><td>Date of Birth</td>
  </tr>
  <tr>
    <td>streetAddress</td><td>Street Address</td>
  </tr>
  <tr>
    <td>city</td><td>City</td>
  </tr>
  <tr>
    <td>state</td><td>State</td>
  </tr>
  <tr>
    <td>zip</td><td>Zip</td>
  </tr>
  <tr>
    <td>homePhone</td><td>Home Phone</td>
  </tr>
</table>

Alerts are an array of possible alerts associated with the search.

<table>
  <tr><th>Field</th><th>Description</th></tr>
  <tr><td>alerts</td><td>Array of alerts</td></tr>
  <tr><td>alert</td><td>single alert message</td></tr>
</table>

Possible alerts:

	The input SSN is reported as deceased
	The input SSN was issued prior to the input date-of-birth
	The input name and SSN are verified but not with the input address and phone
	The input SSN is invalid or not yet issued
	The input phone number may be disconnected
	The input zip code belongs to a post office box
	Unable to verify address
	Unable to verify SSN
	Unable to verify phone number
	Unable to verify date of birth
	The input address may have been miskeyed
	The input phone number may have been miskeyed
	Unable to verify name
	The input SSN is associated with multiple last names
	The input SSN is recently issued
	The input phone area code is changing
	The input address matches a prison address
	The input last name is not associated with the input SSN
	The input first name is not associated with input SSN
	The input work phone is potentially disconnected
	The input SSN is associated with the same first name but a different last name
	The input phone number is associated with a different name and address
	The input name and address are associated with an unlisted/non-published phone number
	The input name may have been miskeyed
	The input SSN was issued to a non-US citizen
	The input SSN was issued within the last three years
	Multiple identities associated with input social
	Multiple SSNs are reported with this name
	The primary input address is a PO Box

Additional Addresses is an array of additional former addresses possibly associated with the search result.

<table>
  <tr><th>Field</th><th>Description</th></tr>
  <tr><td>additionalAddresses</td><td>Array of additional addresses</td></tr>
  <tr><td>address</td><td>Contains address elements</td></tr>
  <tr><td>streetAddress</td><td>Street Address</td></tr>
  <tr><td>cityStateZip</td><td>City State Zip</td></tr>
</table>

Additional Last Names is an array of additional last names possibly associated with the search result.

<table>
  <tr><th>Field</th><th>Description</th></tr>
  <tr><td>additionalLastNames</td><td>Array of additional last names</td></tr>
  <tr><td>additionalLastName</td><td>Contains last name element</td></tr>
  <tr><td>lastName</td><td>Last Name</td></tr>
  <tr><td>dateLastSeen</td><td>City State Zip</td></tr>
</table>

Errors is an array of validation errors or other associated with not being able to produce a result.

<table>
  <tr><th>Field</th><th>Description</th></tr>
  <tr><td>errors</td><td>Array of errors</td></tr>
  <tr><td>error</td><td>Error Message</td></tr>
</table>

Possible errors:

     not found
     daily request limit reached
     IP address not in range
     First name must be supplied
     Last name must be supplied
     SSN must be supplied if Address information or Date of Birth is blank
     Street address1 must be supplied if Date of Birth or Social Security Number is blank
     City and State must be supplied if the Zipcode is blank
     State and City must be supplied if the Zipcode is blank
     Zipcode must be supplied if City and State is blank
     DOB must be supplied if Address information or Social Security Number is blank
     Phone must include area code and be 10 digits long
     SSN must be 9 digits long
     DOB could not be determined from input
     DOB must be in the format MM/DD/YYYY



Example:
```XML

<?xml version="1.0" encoding="UTF-8"?>
<verifySearch>
  <alerts type="array">
    <alert>
      <alert>The input phone number is associated with a different name and address</alert>
    </alert>
  </alerts>
  <firstName>
    <status>verified</status>
    <result>TEST</result>
  </firstName>
  <lastName>
    <status>verified</status>
    <result>User</result>
  </lastName>
  <additionalLastNames type="array">
    <additionalLastName>
      <lastName>Admin</lastName>
      <dateLastSeen>3/2012</dateLastSeen>
    </additionalLastName>
  </additionalLastNames>
  <sSN>
    <status>verified</status>
    <result>12345xxxx</result>
  </sSN>
  <dOB>
    <status>verified</status>
    <result>03/31/1977</result>
  </dOB>
  <streetAddress>
    <status>verified</status>
    <result>104 N. Summit St FL 2</result>
  </streetAddress>
  <city>
    <status>verified</status>
    <result>Toledo</result>
  </city>
  <state>
    <status>not_verified</status>
    <result nil="true"></result>
  </state>
  <zipcode>
    <status>verified</status>
    <result>43604</result>
  </zipcode>
  <additionalAddresses type="array">
    <additionalAddress>
      <address>12345 MAIN ST</address>
      <cityStateZip>GERMANTOWN TX 66087-1234</cityStateZip>
    </additionalAddress>
  </additionalAddresses>
  <homePhone>
    <status>verified</status>
    <result>4192441111</result>
  </homePhone>
</verifySearch>
```
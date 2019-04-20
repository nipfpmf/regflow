# RegFlow
RegFlow is an open initiative to ease digital regulatory filings by
use of open standards and technology.

## Architecture

![](https://raw.githubusercontent.com/nipfpmf/regflow/master/architecture_diagram.png)

## Components

**XBRL Validating Parser** will test incoming XBRL Instance documents
to make sure it is XML Schema valid based on TRAI XBRL Taxonomy. The
parser will also parse all XML field values from the document for
storing them in the database. _Service Layer_ will be responsible for
calling this component and getting back results which will then be
forwarded to _ORM_ for storing in the database.

**API** component will enable other outside systems to interact with
_RegFlow_. This component will contain a set of interfaces which will
be responsible for receiving requests from other systems, forward
these requests to _Service Layer_ for processing, and then responding
to the outside system with an appropriate response.

**Authentication/Authorisation** component will be responsible for
controlling access to _RegFlow_. The component will perform two
functions:

  * **Authentication** will allow only permitted entities to make
    requests to _RegFlow_ API by way of pre-approved users and
    secure keys.

  * **Authorisation** will restrict _authenticated_ entities to
      perform only the operations they are authorised to do
      (separation of roles).

This component will work directly with API component for making sure
no API request is unauthorised. The _Authentication/Authorisation_
should also be configured to set up access controls for users who want
to access publicly shareable data.

**Service Layer** component forms the heart of _RegFlow_ backend layer
by controlling program execution and flow. This component will be
responsible for performing operations forwarded by API component and
_Batch Jobs_ component by making use of other components of
_RegFlow_. Most of the business logic will also be implemented in this
component.

**Object Relational Mapping (ORM)** component will be responsible for
mapping objects _Service Layer_ operations directly with _Primary
Database_ component. This component will be the only component
interacting with _database_ component.

**Primary Database** will be responsible for storing raw data received
from regulated entity filings. The database system should be
performance efficient, and sufficiently large to store various data
coming in from different entities.

**Batch jobs** component will be responsible for running programs
which need to be run at a regular frequency. For example, a program to
fetch all data from database which can be shared with general public
will pull this data out of primary database and put it on regulator's
website.

**Auditing** component will keep a record of all activity happening
inside _RegFlow_, for compliance purposes. This component, thus, as
shown in architecture diagram spans all _RegFlow_ components. All
other components will send relevant information to this component. For
example, a record of logins, API requests, and authorised accesses may
be maintained in this component.

**Logging** component will maintain all application logs. These logs
are useful for debugging application errors, for making sure
application is performing correctly among others.

**Reporting (iXBRL)** component will provide a front-end to view
reports of all regulatory filings data. [_iXBRL 1.1_](https://specifications.xbrl.org/work-product-index-inline-xbrl-inline-xbrl-1.1.html) is an HTML based standard which 
is used for displaying XBRL reports. It enables presentation of XBRL
Facts related to XBRL Concepts along with background text template.

**Data submission application** will be developed and maintained by
regulated entities to submit data to _RegFlow_. This application will
interact with _RegFlow_ via API component.

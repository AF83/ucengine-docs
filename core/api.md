# ReST API

## General informations about the API

The base URL is `http://demo.ucengine.org/api/0.3/`
All the others API URLs are relative to this one.

You have to consider a few conventions :

- The `PUT` and `DELETE` methods can be simulated when your client could not
  support these. To do so, you have to use the `_method` parameter.  For
  example, to be able to do a `PUT` request, you will send a `POST` request
  with the following parameter: `_method=PUT`.

## Authentication

If an API method need authentication, there are recurrent parameters :

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**Encoded Parameters**                 |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`sid`                                  | Session id                            | `330249245470504`

## Time
### Retrieve current server's timestamp

#### Request

    GET /time

#### Returned values

    200 {"result": 1284046082844}

#### Notes

The timestamp is the number of milliseconds elapsed since EPOCH (1970-01-01).

## User
### List users

#### Request

    GET /user/

#### Require authentication:

yes

#### Returned values

    200 {"result": [{   "uid":"romain.gauthier@af83.com",
                        "domain":"ucengine.org",
                        "auth":"password",
                        "metadata":{
                            "nickname":"abel_1284046072075"
                        }
                    },
                    {   "uid":"victor.goya@af83.com",
                        "domain":"ucengine.org",
                        "auth":"password",
                        "metadata":{
                            "nickname":"phorque"
                        }
                    }]}

    401 { "error": "unauthorized" }

    500 { "error": "unexpected_error" }

### Register users

#### Request

    POST /user/

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**Encoded Parameters**                 |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`auth`                                 | Authentication method                 | `password` or `token`
`credential`                           | Password or token                     | `dWlkXzYzNDQ0MzI2NDQzXzUwMTUwCg`
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Returned values:

    201 { "result": "created" }

    401 { "error": "unauthorized" }

    500 { "error": "unexpected_error" }

### Modify user's informations

#### Request

    PUT /user/{uid}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
**Encoded Parameters**                 |                                       |
`auth`                                 | Authentication method                 | `password` or `token`
`credential`                           | Password or token                     | `dWlkXzYzNDQ0MzI2NDQzXzUwMTUwCg`
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Require authentication:

yes

### Returned values

    200 { "result": "ok" }

    401 { "error": "unauthorized" }

    500 { "error": "unexpected_error" }

### Retrieve user's informations

#### Request

    GET /user/{uid}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`

#### Require authentication:

yes

#### Returned values

     200 {"result": {    "uid":"romain.gauthier@af83.com",
                         "domain":"ucengine.org",
                         "auth":"password",
                         "metadata":{
                            "nickname":"abel_1284046072075"
                         }
                    }}

    401 { "error": "unauthorized" }

    404 { "error": "not_found" }

    500 { "error": "unexpected_error" }

### Delete a user

#### Request

    DELETE /user/{uid}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    401 { "error": "unauthorized" }

    404 { "error": "not_found" }

    500 { "error": "unexpected_error" }

## Authentification

#### Request

    POST /presence/

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**Encoded Parameters**                 |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`credential`                           | Password or token                     | `dWlkXzYzNDQ0MzI2NDQzXzUwMTUwCg`
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Returned values

    200 { "result": "409832095702309473209" } // the result is a valid sid

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    403 { "error": "bad_credentials" } // the authentification has failed

## Disconnect users

#### Request

    DELETE /presence/{sid}

#### Paramètres

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`sid`                                  | Session id                            | `330249245470504`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }}.

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to disconnect this presence

    404 { "error": "not_found" } // the presence resource doesn't exists

#### Notes

The user given as an URL parameter is not necessarily the same than the one
passed as an encoded parameter. Thus you can disconnect another user (if the
ACLs allow it of course).

## Infos

### Get current domain informations

#### Request

    GET /infos

#### Returned values

    200 { "result": {"domain": "ucengine.org",
                     "metadata": [{"description", "an useful description"}]} }

### Update current domain informations

#### Request

    PUT /infos

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to updated informations

## Meeting
### Join a meeting

#### Request

    POST /meeting/all/{meeting}/roster/

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
**Encoded Parameters**                 |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to join the meeting

    404 { "error": "not_found" } // the meeting doesn't exists

### Quit a meeting

#### Request

    DELETE /meeting/all/{meeting}/roster/{uid}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
`uid`                                  | User id                               | `uid_63444326443_50150`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to quit the meeting

    404 { "error": "not_found" } // the meeting doesn't exists


### Create a meeting

#### Request

    POST /meeting/all/

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**Optional Encoded Parameters**        |                                       |
`meeting`                              | Meeting id                            | `demo`
`start`                                | opening Timestamp of the meeting      | `63444430100`
`end`                                  | ending Timestamp of the meeting       | `63444430800`
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Require authentication:

yes

#### Notes

- The timestamps are the number of milliseconds elapsed since EPOCH (1970-01-01).
- If the 'end' parameter is missing, the the meeting has no end date.
- if the 'start' and 'end' parameters are missing, the meeting starts
  immediatly and has no end date

#### Returned values

    201 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to create a meeting

    409 { "error": "conflict" } : // the meeting already exists


### Modify a meeting

#### Request

    PUT /meeting/all/{meeting}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
**Optional Encoded Parameters**        |                                       |
`start`                                | opening Timestamp of the meeting      | `63444430100`
`end`                                  | ending Timestamp of the meeting       | `63444430800`
`metadata`                             | Array containing metadata             | `metadata[key]=value`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to delete the meeting

    404 { "error": "not_found" } // the meeting doesn't exists


### List users connected to a meeting (roster)

#### Request

    GET /meeting/all/{meeting}/roster

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`


#### Returned values

    200 {"result":[{    "uid":"abel.fournier_1284046072075@af83.com",
                        "domain":"ucengine.org",
                        "auth":"password",
                        "metadata":{
                                "nickname":"abel_1284046072075"}
                        },
               {        "uid":"abel.fournier_1284107725374@af83.com",
                        "domain":"ucengine.org",
                        "auth":"password",
                        "metadata":{
                                "nickname":"abel_1284107725374"}
                        },
               {        "uid":"abel.fournier_1284114120882@af83.com",
                        "domain":"ucengine.org",
                        "auth":"password",
                        "metadata":{
                                "nickname":"abel_1284114120882"}
               },
               ...
    ]}

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to list the users of this meeting

    404 { "error": "not_found" } // the meeting doesn't exists

### List the meetings

#### Request

    GET /meeting/

    GET /meeting/{status}


Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`status`                               | Status of the meeting                 | `upcoming` or `opened` or `closed` or `all`

#### Require authentication:

yes

#### Returned values

    200 {"result":[{    "name":"demo",
                        "domain":"ucengine.org",
                        "start_date":1284046056927,
                        "end_date":"never",
                        "metadata":{    "description":"U.C.Engine demo meetup"}
                },
                {       "name":"demo2",
                        "domain":"ucengine.org",
                        "start_date":1284046056928,
                        "end_date":"never",
                        "metadata":{"description":"Meeting R&D"}
                },
                {       "name":"agoroom",
                        "domain":"ucengine.org",
                        "start_date":1284046056928,
                        "end_date":1284046056928,
                        "metadata":{"description":"Meeting agoroom"}
                }
    ]}

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to list the meetings

### Retrieve meeting's informations

#### Request

    GET /meeting/all/{meeting}

#### Parameters

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`

#### Require authentication:

yes

#### Returned values

    200 {"result":{     "name":"demo",
                        "domain":"ucengine.org",
                        "start_date":1284046056927,
                        "end_date":"never",
                        "metadata":{    "description":"U.C.Engine demo meetup"}
                }}

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the users is not authorized to retrieve the meeting's informations


### Delete a meeting

#### Request

    DELETE /meeting/all/{meeting}

#### Parameters

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" }

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to delete the meeting

    404 { "error": "not_found" } // the meeting doesn't exists

## Events
### Retrieve the events

#### Request

    GET /event/

    GET /event/{meeting}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
**Optional Encoded Parameters**        |                                       |
`type`                                 | The event's type                      | `internal.meeting.add`
`start`                                | Start of the event's frame            | `63444430100`
`end`                                  | End of the event's frame              | `63444430300`
`from`                                 | The sender of the event               | `uid_63444326443_50150`
`count`                                | Number of events to return            | `42`
`page`                                 |                                       |
`order`                                |                                       |
`search`                               |                                       |
`parent`                               |                                       |
`_async`                               | Method used to retrieve the events    | `no` or `lp`

#### Require authentication:

yes

#### Returned values

    200 {"result": [{   "type":"join_meeting_event",
                        "domain":"ucengine.org",
                        "datetime":1284046079374,
                        "id":"24653994823933231622695570265810",
                        "location":"demo",
                        "from":"abel.fournier_1284046072075@af83.com",
                        "metadata":{}
                },
                {       "type":"post_annotation_event",
                        "domain":"ucengine.org",
                        "datetime":1284046082844,
                        "id":"20196912711920626263917946711292",
                        "location":"demo",
                        "from":"abel.fournier_1284046072075@af83.com",
                        "metadata":{    "language":"fr",
                                        "text":"coucou"}
                },
                {       "type":"translate_annotation_event",
                        "domain":"ucengine.org",
                        "datetime":1284046083272,
                        "id":"61614248092678409569587739330424",
                        "location":"demo",
                        "from":"abel.fournier_1284046072075@af83.com",
                        "metadata":{    "traduction":"cuckoo",
                                        "language":"en"}
                },
                ...
    ]}

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to list the events of this meeting

    404 { "error": "not_found" } // the meeting doesn't exists

#### Notes

'start' and 'end' parameters allow you to frame the events :

- If 'end' is missing: return all the events from `start` to the end of the timeline.
- If 'start' and 'end' are missing: return all the events of the timeline.
- The `limit` option can be negative. For instance, `limit=-2` will return the 2 last events of the frame.
  Limit can also take the `last` value which is equivalent to `limit=-1`
- The `_async` option allow you to return the events to the client in real time.

### Send an event to U.C.Engine

#### Request

    POST /event/

    POST /event/{meeting}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
**Encoded Parameters**                 |                                       |
`type`                                 | The event's type                      | `internal.meeting.add`
**Optional Encoded Parameters**        |                                       |
Any other parameter                    | These will be part of the metadata    |

#### Require authentication:

yes

#### Returned values

    201 {"result": "24653994823933231622695570265810"}

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to send events to this meeting

    404 { "error": "not_found" } // the meeting doesn't exists

## Files
### Upload a file

#### Request

    POST /file/{meeting}


Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
**Optional Encoded Parameters**        |                                       |
`_filename`                            | Filename                              | `ucengine.odp`

#### Require authentication:

yes

#### Returned values

    201 { "result": "ucengine_4534543543.odp"} // the file id

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to upload files in this meeting

    404 { "error": "not_found" } // the meeting doesn't exists

#### Notes

- You are able to force the filename if it is specified in the url or in
  the request's body (this second case is often handled by the browser).
- To avoid conflicts between files U.C.Engine generate a unique id which is returned in the result.

### List files

#### Request

    GET /file/{meeting}

#### Parameters

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`

#### Require authentication:

yes

#### Returned values

    200 { "result": [ { "filename": "U.C.Engine.odp"
                        "domain": "ucengine.org",
                      , "token": "42314657480629893636680972"
                      , "description": "Présentation du projet U.C.Engine"
                      }
                    , ...
                    ]}

    401 { "error": "unauthorized" }

    500 { "error": "unexpected_error" }

### Download a file

#### Request

    GET /file/{meeting}/{filename}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`meeting`                              | Meeting id                            | `demo`
`filename`                             | Filename                              | `ucengine.odp`

#### Require authentication:

yes

#### Returned values

    200 The file to download

    401 { "error": "unauthorized" }

    404 { "error": "not_found" }

    500 { "error": "unexpected_error" }


## ACLs

### Check user's rights

#### Request

    GET /user/{uid}/acl/{object}/{action}

    GET /user/{uid}/acl/{object}/{action}/{meeting}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`object`                               | The object on which the right apply   | `meeting`  or `event`
`action`                               | Authorized action for this right      | `add` or `deletè or `join` ...
`meeting`                              | Meeting id                            | `demo`
**Encoded Parameters**                 |                                       |
`condition`                            | Array of conditions to satisfy        |

#### Require authentication:

yes

#### Examples

If the 'romain' user wants to check that the 'toto' user has the right to join the 'ucengine' meeting, the request will be :

        GET /user/toto/acl/meeting/join/ucengine?uid=romain&sid=40324302840329843809543

#### Returned values

    200 {"result": "true"}

    200 {"result": "false"}

    400 { "error": "bad_parameters" } : au moins un paramètre est manquant ou erroné ;

    401 { "error": "unauthorized" } : l'utilisateur n'est pas authorisé à vérifier les droits de cet utilisateur;

    404 { "error": "not_found" } : l'utilisateur n'existe pas

### Add a right to a user

#### Request

    POST /user/{uid}/acl/{object}/{action}

    POST /user/{uid}/acl/{object}/{action}/{meeting}

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`object`                               | The object on which the right apply   | `meeting` or `event`
`action`                               | Authorized action for this right      | `add` or `deletè or `join` ...
`meeting`                              | Meeting id                            | `demo`
**Encoded Parameters**                 |                                       |
`condition`                            | Array of conditions to satisfy        |

#### Require authentication:

yes

#### Returned values

    201 { "result": "ok" } // the right has been successfully added

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to add this right

    404 { "error": "not_found" } // the user doesn't exists

### Delete a right

#### Request

    DELETE /acl/{uid}/{object}/{action}

    DELETE /acl/{uid}/{object}/{action}/{meeting}

#### Parameters

Parameter                              | Description                           | Example
---------------------------------------|---------------------------------------|------------------------------------------------------------
**URL Parameters**                     |                                       |
`uid`                                  | User id                               | `uid_63444326443_50150`
`object`                               | The object on which the right apply   | `meeting` or `event` ...
`action`                               | Authorized action for this right      | `add` or `deletè or `join` ...
`meeting`                              | Meeting id                            | `demo`
**Encoded Parameters**                 |                                       |
`condition`                            | Array of conditions to satisfy        |

#### Require authentication:

yes

#### Returned values

    200 { "result": "ok" } // the right has been successfully deleted

    400 { "error": "bad_parameters" } // at least one paremeter is missing or wrong

    401 { "error": "unauthorized" } // the user is not authorized to delete this right

    404 { "error", "not_found" } // the users doesn't exists

# README

## Task
Call center software: There is a call center with employees that have certain expertise areas; customers are calling and indicate their desired area.

You have to write the server that directs the calls (binds customers to employees) maximizing the number of customer contacts that are fulfilled.

The server should handle 3 types of requests:
# `http://server.name/register?name=EmployeeName1&area=bills&area=contracts&area=special-offers`
This request defines EmployeeName1 as an employee with three areas of expertise: bills,contracts and special-offers. Only one employee can be defined in a request. The same name can be redefined multiple times, because employees are becoming available after handling a
customer. The response must be code 200, with content: `WELCOME`

# http://server.name/call?area=bills&area=leases&area=bills
This indicates that a new batch of 3 customers are on the line. Two of them are interested in "bills" and one in "leases". Based on the list of registered employees, you have to assign (or decline) each call, in the order they appear in request. You have to respond with HTTP code
200 OK, and content is a JSON indicating the assigned employees. For example:
```json
{
  "totalAssignments" : "1",
  "assignments" : [
                  {
                    "area" : "bills",
                    "employee" : "EmployeeName1"
                  },
                  {
                    "area" : "leases",
                    "employee" : ""
                  },
                  {
                    "employee" : "",
                    "area" : "bills"
                  }
                  ]
}
```
The reply above indicates that only one assignment was possible: EmployeeName1 takes the
first call about "bills". The other calls should be postponed because no employee is available.
This is indicated by empty "employee" value. Once an assignment is set, the employees listed
are busy taking the call, so they are no longer available. They may return later (via a register
request).
# http://server.name/reset
This is done usually at the end of a work day. It means that all registered employees leave
home: the system can restart tomorrow.

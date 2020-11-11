# 12_Survey

Unsecure data processing for getting information(the survey)

## Method

Go to the survey page (<http://192.168.1.102/?page=survey#>)
Inspect element
We are in front of a table, with lots of forms, without any validation (neither in front nor in back). You just have to change one of the values of a field by a value you want for example: 10,000 which will give 10,000 points (enough to distort the survey)
This will give us the flag: 03A944B434D5BAFF05F46C4BEDE5792551A2595574BCAFC9A6E25F67C382CCAA

## Manipulation

Change the value and vote for your favorite option whenever there is a poll
THis way you can make sure your option always wins the poll

## Prevention

Use validation on backend as well and send an appropriate response if the front-end has been tampered with

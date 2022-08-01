# public api

## routes 

#### GET api.juniordoctors.co.uk/public/v1/departmentreviews

Purpose:
To retrieve reviews for a given department

Parameters:

  **key** (*Mandatory*): Your unique UUID string

  **site** (*Mandatory*): Unique UUID for the site you are requesting reviews for. Only one site can be included per request

  **speciality** (*Mandatory*): Unique UUID for the department speciality you are requesting reviews for.
  
  **sortby** (*Optional - defaults to "T"*): 
     Options include:
       - "T" (sorts reviews by most recent first)
       - "H" (sorts reviews by highest rated first)
       - "L" (sort reviews by lowest rated first)
    
  **offset** (*Optional - defaults to 0*): Integer value for offsetting reviews returned (for pagination) 
  

  
  Example:
  
  ```
  
   Generic example:
   
   https://api.juniordoctors.co.uk/public/v1/departmentreviews?token={**INSERT TOKEN HERE**}&site={**INSERT SITE UUID HERE**}&speciality={**INSERT DEPARTMENT SPECIALITY HERE**}&sortby={**INSERT SORT BY PREFERENCE HERE**}&offsetby={**INSERT OFFSET VALUE HERE**}&istest=true
   
   Example #1:
   
   https://api.juniordoctors.co.uk/public/v1/departmentreviews?token=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&site=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&speciality=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&sortby=H&offsetby=0&istest=true
  
  ```
  
Returns:

Valid Request: Reviews Object

Example: 

 ```
 
   Reviews Object with types:
   
   {
      
      site: string (name of site in a displayable format),
      department: string (name of department in a displayable format),
      averageRating: numeric (average rating for this department - fixed to one decimal place),
      reviewCount: numeric (total number of reviews available for this department),
      reviews: Review[]

   }
   
   Review Object with types:
   
   {
      review: string
      site: string (name of site in a displayable format)
      department: string (name of department in a displayable format)
      programme: string (name of programme in a displayable format)
      yearPosted: numeric (year review was posted)
      rating: numeric (rating 0-5)
   }
  
   Full Example:
   
   {
    
      site: 'Generic Royal Infirmary',
      department: 'Anaesthetics',
      averageRating: 5.0,
      reviewCount: 1,
      reviews: [
        { 
         review: 'I loved this job, was my favourite placement I did. All the consultants we're super supportive and I always
          left on time, would highly recommend!',
         site: 'Generic Royal Infirmary',
         department: 'Anaesthetics',
         programme: 'Core Anaesthetics',
         yearPosted: 2022
         rating: 5,
         workHereAgain: true
       }
    
     ]
   
   }
  
  ```
  
  Invalid Request: 400
  
#### POST api.juniordoctors.co.uk/public/v1/departmentreview

Purpose:
To post a new review
  
Submit as JSON `{ review: {REVIEW OBJECT} }`

Review Object Parameters:
    
**key** (*Mandatory*): Your unique UUID string

**site** (*Mandatory*): Unique UUID for the site you are requesting reviews for. Only one site can be included per request
    
**speciality** (*Mandatory*): Unique UUID for the department speciality you are requesting reviews for.

**programme** (*Mandatory*): Unique UUID for the reviewers training programme / job type.
    
**review** (*Mandatory*): The review itself
      - must be at least 40 characters (otherwise request will return 400)
      - will be automatically scanned for abuse - abusive reviews will return 200 but will flag for moderation
      
**rating** (*Mandatory*): Integer from 0 to 5. Anything out of this range will return 400

**workHereAgain** (*Mandatory*): Boolean. True for 'yes I would work here again', false for 'I would not work here again' 
        
**!!IMPORTANT!!**    
        
**isTest** (*Mandatory - defaults to true*): Boolean. If true will return 200 if valid review and request but will not save the review - useful for testing. Make sure to set to false in production or reviews will not be saved
 
```
Example request body:

review: {
  key: 'd60c3e80-4e4c-45b5-a4d4-b42ce12059b7',
  site: '24648cf6-4b60-456c-8b19-1d824dfd0e68',
  speciality: 'cec17531-efc8-47e1-a462-c0d478af526d'
  programme: '77c7af33-2cf5-4405-b2ca-b7799b0b55d2',
  review: 'I loved this job, was my favourite placement I did. All the consultants we're super supportive and I always
  left on time, would highly recommend!',
  rating: 5,
  workHereAgain: true,
  isTest: true
}

```

Returns: 

Valid Request: Review Object

Example: 

 ```
   
   Review Object with types:
   
   {
      review: string
      site: string (name of site in a displayable format)
      department: string (name of department in a displayable format)
      programme: string (name of programme in a displayable format)
      yearPosted: numeric (year review was posted)
      rating: numeric (rating 0-5)
   }
   
   Full Example:
   
   { 
      review: 'I loved this job, was my favourite placement I did. All the consultants we're super supportive and I always
       left on time, would highly recommend!',
      site: 'Generic Royal Infirmary',
      department: 'Anaesthetics',
      programme: 'Core Anaesthetics',
      yearPosted: 2022
      rating: 5,
      workHereAgain: true
    }
   
   ```
   
  ## Programmes
  
  Useful list of programmes to test out
  
  Programme Name         | UUID 
  ---------------------- | -------------------------------------
  Anaesthetics ST4+      | 1b0b5255-5774-4668-8e72-ca541abe8b6d
  Core Anaesthetics      | 18f1c796-ac33-47a7-99f3-7fe3f5ef4f8a
  Foundation Programme   | 18f1c796-ac33-47a7-99f3-7fe3f5ef4f8a
  Junior Clinical Fellow | 2c040b66-a8b9-4531-a436-78943b1652f8
  Senior Clinical Fellow | f1bb37cd-d3bc-457a-9b9d-aa6d075a7633


 ## Specialities
 
 Speciality Name | UUID
 ----------------|-------------
 Anaesthetics    | 63db4685-7c24-409d-b561-6db50fc4ccc2
  

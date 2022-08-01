# public api

## routes (not live yet)

#### GET api.juniordoctors.co.uk/public/v1/reviews

Purpose:
To retrieve reviews for a given department

Parameters:

  **token** (*Mandatory*): Your unique UUID string (to request email: hello@juniordoctors.co.uk)

  **site** (*Mandatory*): Unique UUID for the site you are requesting reviews for. Only one site can be included per request

  **speciality** (*Mandatory*): Unique UUID for the department speciality you are requesting reviews for.
  
  **sortby** (*Optional - defaults to "T"*): 
     Options include:
       - "T" (sorts reviews by most recent first)
       - "H" (sorts reviews by highest rated first)
       - "L" (sort reviews by lowest rated first)
    
  **offset** (*Optional - defaults to 0*): Integer value for offsetting reviews returned (for pagination) 
  
  **!!IMPORTANT!!**    
        
  **istest** (*Mandatory - defaults to true*): Boolean. If true will return 200 if valid review and request but will not save the review - useful for testing. Make sure to set to false in production or reviews will not be saved
  
  Example:
  
  ```
  
   Generic example:
   
   https://api.juniordoctors.co.uk/public/v1/reviews?token={**INSERT TOKEN HERE**}&site={**INSERT SITE UUID HERE**}&speciality={**INSERT DEPARTMENT SPECIALITY HERE**}&sortby={**INSERT SORT BY PREFERENCE HERE**}&offsetby={**INSERT OFFSET VALUE HERE**}&istest=true
   
   Example #1:
   
   https://api.juniordoctors.co.uk/public/v1/reviews?token=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&site=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&speciality=d60c3e80-4e4c-45b5-a4d4-b42ce12059b7&sortby=H&offsetby=0&istest=true
  
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
   
   Review with types:
   
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
   
   
   Review Type:
   {
      review: string
      site: string (name of site in a displayable format)
      department: string (name of department in a displayable format)
      programme: string (name of programme in a displayable format)
      yearPosted: numeric (year review was posted)
      rating: numeric (rating 0-5)
      
   
   }
  
  ```
  
#### POST api.juniordoctors.co.uk/public/v1/review
  
Parameters:
    
**token** (*Mandatory*): Your unique UUID string (to request email: hello@juniordoctors.co.uk)

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


{
  token: 'd60c3e80-4e4c-45b5-a4d4-b42ce12059b7',
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
     

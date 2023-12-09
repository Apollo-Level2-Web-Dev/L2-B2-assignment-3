# **Assignment 3**

# **Assignment Requirements:**

### **Technology Stack:**

- **Programming Language:** TypeScript
- **Web Framework:** Express.js
- **Object Data Modeling (ODM) and Validation Library:** Mongoose for MongoDB

## **Models:**

### **1. Course Model:**

- **Fields:**
    - **title (String):** A **unique** title of the course.
    - **instructor (String):** The instructor of the course.
    - **categoryId (Object ID):** A reference to the category collection.
    - **price (Number):** The price of the course.
    - **tags(Array of Object):** The "tags" field is an array of objects, each having a "name" (string) and "isDeleted" (boolean) property.
    - **startDate (String):** The start date of the course.
    - **endDate (String):** The end date of the course.
    - **language (String):** The language in which the course is conducted.
    - **provider (String):** The provider of the course.
    - **durationInWeeks (Integer):** The total duration of the course in weeks (duration should must be in integer. Floating point number not allowed).
    - **details (Object):**
        - l**evel (string):** e.g., Beginner, Intermediate, Advanced.
        - **description (string):** Detailed description of the course

### **2. Category Model:**

- **Fields:**
    - **name (String):** A unique name of the category.

### **3. Review Model:**

- **Fields:**
    - **courseId (Object ID):** A reference to the course collection.
    - **rating (Number):** The user's rating, which falls within the range of 1 to 5.
    - **comment (String):** The comment or review text provided by the user.

## **Error Handling:**

Implement proper error handling throughout the application. Use global error handling middleware to catch and handle errors, providing appropriate error responses with status codes and error messages.

### **Error Response Object:**

- **success:** false
- **message:** Error Type → Validation Error, Cast Error, Duplicate Entry
- **errorMessage**: A concise error message. Concatenated string containing detailed error information.
- **errorDetails:**: Detailed information about the error
- **stack**: Stack trace for debugging purposes

### **Sample Error Response:**

- For Cast Error
```json
{
    "success": false,
    "message": "Invalid ID",
    "errorMessage": "656dce0f133c4a8a53eb is not a valid ID!",
    "errorDetails": {
        "stringValue": "656dce0f133c4a8a53eb",
        "valueType": "string",
        "kind": "ObjectId",
        "value": "656dce0f133c4a8a53eb",
        "path": "_id",
        "reason": {},
        "name": "CastError",
        "message": "Cast to ObjectId failed for value '656dce0f133c4a8a53eb' (type string) at path '_id' for model 'Faculty'"
    },
    "stack": "CastError: Cast to ObjectId failed for value '656dce0f133c4a8a53eb' (type string) at path '_id' for model 'Faculty'\n    at SchemaObjectId.cast (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\schema\\objectId.js:250:11)\n    at SchemaObjectId.SchemaType.applySetters (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\schemaType.js:1219:12)\n    at SchemaObjectId.SchemaType.castForQuery (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\schemaType.js:1633:15)\n    at cast (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\cast.js:375:32)\n    at model.Query.Query.cast (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\query.js:4768:12)\n    at model.Query.Query._castConditions (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\query.js:2200:10)\n    at model.Query._findOne (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\query.js:2484:8)\n    at model.Query.exec (F:\\level2\\first-project\\node_modules\\mongoose\\lib\\query.js:4290:80)\n    at processTicksAndRejections (node:internal/process/task_queues:95:5)"
}
```

- For Validation Error
```json
{
    "success": false,
    "message": "Validation Error",
    "errorMessage": "gender is required. email is required.",
    "err": {
        "issues": [
            {
                "expected": "'male' | 'female' | 'other'",
                "received": "undefined",
                "code": "invalid_type",
                "path": [
                    "body",
                    "faculty",
                    "gender"
                ],
                "message": "Required"
            },
            {
                "code": "invalid_type",
                "expected": "string",
                "received": "undefined",
                "path": [
                    "body",
                    "faculty",
                    "email"
                ],
                "message": "Required"
            }
        ],
        "name": "ZodError"
    },
    "stack": "ZodError: [\n  {\n    "expected": "'male' | 'female' | 'other'",\n    "received": "undefined",\n    "code": "invalid_type",\n    "path": [\n      "body",\n      "faculty",\n      "gender"\n    ],\n    "message": "Required"\n  },\n  {\n    "code": "invalid_type",\n    "expected": "string",\n    "received": "undefined",\n    "path": [\n      "body",\n      "faculty",\n      "email"\n    ],\n    "message": "Required"\n  }\n]\n    at Object.get error [as error] (F:\level2\first-project\node_modules\zod\lib\types.js:43:31)\n    at ZodObject.parseAsync (F:\level2\first-project\node_modules\zod\lib\types.js:166:22)\n    at processTicksAndRejections (node:internal/process/task_queues:95:5)"
}
```

## **Endpoints:**

### **1. Create a Course**

- **Endpoint:** **`POST /api/course`**
- **Request Body:**
    
```json
{
    "title": "Sample Course",
    "instructor": "Jane Doe",
    "categoryId": "123456789012345678901234",
    "price": 49.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-01-15",
    "endDate":"2023-03-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 12,
    "details": {
        "level": "Intermediate",
        "description": "Detailed description of the course"
    }
}
```
    
- **Response:**
```json
{
    "success": true,
    "statusCode": 201,
    "message": "Course created successfully",
    "data": {
        "title": "Sample Course",
        "instructor": "Jane Doe",
        "categoryId": "123456789012345678901234",
        "price": 49.99,
        "tags": [
            {
                "name": "Programming",
                "isDeleted": false
            },
            {
                "name": "Web Development",
                "isDeleted": false
            }
        ],
        "startDate": "2023-01-15",
        "endDate":"2023-03-14",
        "language": "English",
        "provider": "Tech Academy",
        "durationInWeeks": 12,
        "details": {
            "level": "Intermediate",
            "description": "Detailed description of the course",
        }
    }
}
```
    

### 2. Get Paginated and Filtered Courses. Don’t use the query builder technique which is shown in the module. Use your own implementation for pagination & filtering.

- Feel free to utilize raw queries or aggregations. You have the option to search on Google, explore YouTube, read various blogs and articles, or seek assistance from ChatGPT or other resources.

- **Endpoint:** **`GET /api/courses`**
#### Query Parameters for API Requests:

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

- page: (Optional) Specifies the page number for paginated results. Default is 1.
  Example: ?page=2
  
- limit: (Optional) Sets the number of items per page. Default is a predefined limit.
  Example: ?limit=10

- sortBy: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `title`, `price`, `startDate`, `endDate`, `language`, `duration`.
  Example: ?sortBy=startDate

- sortOrder: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending).
  Example: ?sortOrder=desc

- minPrice, maxPrice: (Optional) Filters results by a price range.
  Example: ?minPrice=20.00&maxPrice=50.00

- tags: (Optional) Filters results by the name of a specific tag.
  Example: ?tags=Programming

- startDate, endDate: (Optional) Filters results by a date range.
  Example: ?startDate=2023-01-01&endDate=2023-12-31

- language: (Optional) Filters results by the language of the course.
  Example: ?language=English

- provider: (Optional) Filters results by the course provider.
  Example: ?provider=Tech Academy

- durationInWeeks: (Optional) Filters results by the duration of the course in weeks.
  Example: ?durationInWeeks=8

- level: (Optional) Filters results by the difficulty level of the course.
  Example: ?level=Intermediate

- **Response:**
    
```json
{
    "success": true,
    "statusCode": 200,
    "message": "Courses retrieved successfully",
    "meta": {
        "page": 1,
        "limit": 10,
        "total": 50
    },
    "data": [
        {
            "title": "Sample Course",
            "instructor": "Jane Doe",
            "categoryId": "123456789012345678901234",
            "price": 49.99,
            "tags": [
                {
                    "name": "Programming",
                    "isDeleted": false
                },
                {
                    "name": "Web Development",
                    "isDeleted": false
                }
            ],
            "startDate": "2023-01-15",
            "endDate":"2023-03-14",
            "language": "English",
            "provider": "Tech Academy",
            "durationInWeeks": 12,
            "details": {
                "level": "Intermediate",
                "description": "Detailed description of the course",
            }
        },
        // more courses
    ]
}
```
    

### 3. Create a Category

- **Endpoint:** **`POST /api/categories`**
- **Request Body:**
    
```json
{
    "name": "Programming"
}
```
    
- **Response:**
```json
{
    "success": true,
    "statusCode": 201,
    "message": "Category created successfully",
    "data": {
        "name": "Programming"
    }
}
```
    

### 4. Get All Categories

- **Endpoint:** **`GET /api/categories`**
- **Response:**
    
```json
{
    "success": true,
    "statusCode": 200,
    "message": "Categories retrieved successfully",
    "data": [
        {
            "name": "Programming"
        },
        // more categories
    ]
}
```
    

### 5. Create a Review

- **Endpoint:** **`POST /api/reviews`**
- **Request Body:**
    
```json
{
    "courseId": "123456789012345678901234",
    "rating": 4,
    "comment": "Great course!"
}
```
    
- **Response:**
    
```json
{
    "success": true,
    "statusCode": 201,
    "message": "Review created successfully",
    "data": {
        "courseId": "123456789012345678901234",
        "rating": 4,
        "comment": "Great course!"
    }
}
```
    

### 

### 6. Update a Course (Partial Update with Dynamic Update)**

- **Endpoint:** **`PUT /api/courses/:courseId`**
- **Request Body:**
    - You can send the partial body data to update the fields you want to update or the full data if you want to update every field of a course. Ensure dynamic updating for both primitive and non-primitive data to prevent the mutation of non-primitive data. [click here for more details](#example-of-updating-primitive-data)
    
```json
{
    "title": "Updated Title",
    "instructor": "New Instructor",
    "categoryId": "123456789012345678901234",
    "price": 59.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": true
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-02-01",
    "endDate":"2023-03-14",
    "language": "Spanish",
    "provider": "Code Masters",
    "durationInWeeks": 16,
    "details": {
        "level": "Intermediate",
        "description": "Detailed description of the course"
    }
}
```
    
- **Response:**
    
```json
{
    "success": true,
    "statusCode": 200,
    "message": "Course updated successfully",
    "data": {
        "title": "Updated Title",
        "instructor": "New Instructor",
        "categoryId": "123456789012345678901234",
        "price": 59.99,
        "tags": [
            {
                "name": "Programming",
                "isDeleted": true
            },
            {
                "name": "Web Development",
                "isDeleted": false
            }
        ],
        "startDate": "2023-02-01",
        "endDate":"2023-11-14",
        "language": "Spanish",
        "provider": "Code Masters",
        "durationInWeeks": 16,
        "details": {
            "level": "Intermediate",
            "description": "Detailed description of the course"
        }
    }
}
```
    

### 7. Get Course by ID with Reviews**

- **Endpoint:** **`GET /api/courses/:courseId/reviews`**
- **Response:**
    
```json
{
    "success": true,
    "statusCode": 200,
    "message": "Course and Reviews retrieved successfully",
    "data": {
        "course": {
            "title": "Updated Title",
            "instructor": "New Instructor",
            "categoryId": "123456789012345678901234",
            "price": 59.99,
            "tags": [
                {
                    "name": "Programming",
                    "isDeleted": false
                },
                {
                    "name": "Web Development",
                    "isDeleted": false
                }
            ],
            "startDate": "2023-02-01",
            "endDate":"2023-11-14",
            "language": "Spanish",
            "provider": "Code Masters",
            "durationInWeeks": 16,
            "details": {
                "level": "Intermediate",
                "description": "Detailed description of the course"
            }
        },
        "reviews": [
            {
                "courseId": "123456789012345678901234",
                "rating": 5,
                "comment": "Awesome course!"
            },
            {
                "courseId": "123456789012345678901234",
                "rating": 4,
                "comment": "Great content!"
            }
            // Additional reviews
        ]
    }
}  
```
    
### 8. Get the Best Course Based on Average Review**
    
- **Endpoint:** **`GET /api/course/best`**
- **Response:**
    - The response includes details about the course, its average rating, and the total number of reviews
    
```json
{
    "success": true,
    "statusCode": 200,
    "message": "Best course retrieved successfully",
    "data": {
        "course": {
            "title": "Best Book Title",
            "instructor": "New Instructor",
            "categoryId": "123456789012345678901234",
            "price": 59.99,
            "tags": [
                {
                    "name": "Programming",
                    "isDeleted": false
                },
                {
                    "name": "Web Development",
                    "isDeleted": false
                }
            ],
            "startDate": "2023-02-01",
            "endDate":"2023-11-14",
            "language": "Spanish",
            "provider": "Code Masters",
            "durationInWeeks": 16,
            "details": {
                "level": "Intermediate",
                "description": "Detailed description of the course"
            }
        },
        "averageRating": 4.8,
        "reviewCount": 50
    }
}
```
    
### **Example of Updating Primitive Data:**
    
Suppose we have a course with the following details:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}
```
    
Now, the client wants to update the price of the course. The request body for the partial update might look like this:
    
```json
{
    "price": 39.99
} 
```
    
In this case, we are updating a primitive data type (price), and it's straightforward. The new course details after the update will be:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 39.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}
```
    
### **Example of Updating Non-Primitive Data (Using "level" in "details"):**
    
Suppose we have a course with the following details:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}
```
    
Now, the client wants to update the "level" of the course. The request body for the partial update might look like this:
    
```json
{
    "details": {
        "level": "Intermediate"
    }
}
```
    
In this case, we are updating a non-primitive data type ("details" object), specifically the "level" field within it. The server should handle dynamic updates for non-primitive data correctly, preserving the existing fields while updating only the specified fields.
    
After the update, the new course details would be:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Intermediate", // Updated value
        "description": "An introductory course to programming basics",
    }
}
```

This ensures that only the specified field within the "details" object is updated, and the rest of the fields remain unchanged. It's a crucial aspect of maintaining data consistency when dealing with non-primitive data structures.
    
### **Example of Updating Both Primitive and Non-Primitive Data:**
    
Suppose we have a course with the following details:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}    
```
    
Now, the client wants to update both the "price" and the "level" of the course. The request body for the partial update might look like this:
    
```json
{
    "price": 39.99,
    "details": {
        "level": "Intermediate"
    }
}
```
    
In this case, we are updating a primitive data type ("price") and a non-primitive data type ("level" within "details") simultaneously. The server should handle dynamic updates for both types correctly.
    
After the update, the new course details would be:
    
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 39.99, // Updated value
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Intermediate", // Updated value
        "description": "An introductory course to programming basics",
    }
}
```
    
This ensures that both the specified primitive field ("price") and the non-primitive field ("level" within "details") are updated independently, and the rest of the details remain unchanged. It's essential for maintaining data consistency when updating a mix of primitive and non-primitive data in a complex data structure.

## Example of updating tags:
- Suppose we have a course with the following tags:
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        {
            "name": "Programming",
            "isDeleted": false
        },
        {
            "name": "Web Development",
            "isDeleted": false
        }
    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}  
```

- Now if we make a request for updating the tag with the below data
```json
{
    "tags": [
        // We want to delete this tag
        {
            "name": "Programming",
            "isDeleted": true
        },
        // We want to add this new tag
        {
            "name": "Mern Development",
            "isDeleted": false
        }
    ],
  other fields you want to update... 

}
```

- After the update, the tags will be modified as follows
```json
{
    "title": "Programming Basics",
    "instructor": "John Doe",
    "price": 29.99,
    "tags": [
        
        {
            "name": "Web Development",
            "isDeleted": false
        },
        {
            "name": "Mern Development",
            "isDeleted": false
        }

    ],
    "startDate": "2023-03-15",
    "endDate":"2023-11-14",
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Beginner",
        "description": "An introductory course to programming basics",
    }
}
```


## Validation with Joi/Zod

- Use Joi/Zod to validate incoming data for user and order creation and updating operations.
- Ensure that the data adheres to the structure defined in the models.
- Handle validation errors gracefully and provide meaningful error messages in the API responses.

## Instruction

1. **Coding Quality:**
    - Write clean, modular, and well-organized code.
    - Follow consistent naming conventions for variables, functions, and routes.
    - Use meaningful names that reflect the purpose of variables and functions.
    - Ensure that the code is readable.
2. **Comments:**
    - Try to provide inline comments to explain complex sections of code or logic.
3. **API Endpoint Adherence:**
    - Strictly follow the provided API endpoint structure and naming conventions.
    - Ensure that the request and response formats match the specifications outlined in the assignment.
4. **Validation and Error Handling:**
    - Implement validation using Joi/zod for both user and order data.
    - Handle validation errors gracefully and provide meaningful error messages in the API responses.
    - Implement error handling for scenarios like user not found, validation errors.
5. **Coding Tools and Libraries:**
    - Avoid the use of AI tools or libraries for generating code. Write the code manually to demonstrate a clear understanding of the concepts.
    - Utilize only the specified libraries like Express, Mongoose, Joi and avoid unnecessary dependencies.
6. **Coding Style:**
    - Consider using linting tools (e.g., ESLint) to enforce coding style and identify potential issues.
    - Ensure there are at least 10 commits in your GitHub repository.

***Not following the specified API endpoint structure, naming conventions, and other instructions will result in a deduction of marks.***

### **Submission:**

- Share the GitHub repository link and the live deployment link as part of your submission.
- Include a README file with clear instructions on how to run the application locally.

### **Deadline:**

- 60 marks: December 13, 2023 11.59PM
- 50 marks: December 14, 2023 11.59PM
- 30 marks: After December 14, 2023 11.59PM

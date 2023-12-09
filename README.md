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
- **errorMessages:** →Use concatenation for concat the error string
- **stack**

### **Sample Error Response:**

```json
{
    "success": false,
    "message": "Duplicate Key Error!",
    "errorMessages": [
        {
            "path": "",
            "message": "E11000 duplicate key error collection: academy.courses index: title_1 dup key: { title: \\"Sample Course\\" }"
        }
    ],
    "stack": "MongoServerError: E11000 duplicate key error collection: academy.courses index: title_1 dup key: { title: \\"Sample Course\\" }\\n    at..."
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
- **Query Parameters:**
    - page, limit, sortBy, sortOrder, minPrice, maxPrice, tags(name of the tag), startDate, endDate, language, provider, durationInWeeks, and level.
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
    
In this case, we are updating a non-primitive data type ("details" object), specifically the "level" field within it. The server should handle dynamic updates for non-primitive data correctly, preserving the existing details while updating only the specified fields.
    
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
    "language": "English",
    "provider": "Tech Academy",
    "durationInWeeks": 8,
    "details": {
        "level": "Intermediate", // Updated value
        "description": "An introductory course to programming basics",
    }
}
```

This ensures that only the specified field within the "details" object is updated, and the rest of the details remain unchanged. It's a crucial aspect of maintaining data consistency when dealing with non-primitive data structures.
    
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

## Validation with Joi/Zod

- Use Joi/zod to validate incoming data for user and order creation and updating operations.
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

- 60 marks: 
- 50 marks: 
- 30 marks: 

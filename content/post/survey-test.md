+++
date = '2025-02-21T10:58:17+05:30'
draft = false
title = 'Survey Test'
pin = true
+++

# Survey API - Testing Instructions
<!--more-->
This README provides instructions on how to test the Survey API endpoints.  The API allows users to create, update, and retrieve surveys, including support for conditional questions.

## Base URL

All API endpoints are accessed under the base URL: `/api`

## Authentication

All endpoints require authentication. You will need to include a valid JWT (JSON Web Token) in the `Authorization` header of your requests.  This token is typically obtained after user login.  Example:

```
Authorization: Bearer <your_jwt_token>
```

## Endpoints

### 1. Create Survey (POST /api/surveys)

Creates a new survey.

**Request Body:**

```json
{
  "title": "Survey Title",
  "type": "nps", //nps or other
  "questions": [
    {
      "question": "Question 1 Text",
      "type": "rating",
      "options": ["1", "2", "3", "4", "5"], // Optional for some question types
      "isSkip": false, // Optional
      "conditional_logic": { // Optional
        "condition": "equals",
        "expected_value": "2",
        "previous_question_id": "question_1" // Key referencing the question
      }
    },
    // ... more questions
  ]
}
```

*   `type`: Must be one of: "nps", "other", or "email".
*   `questions`: An array of question objects.
*   `conditional_logic`:  Optional.  If present, `previous_question_id` must reference a question key (e.g., "question_1", "question_2"). These keys are assigned based on the order of the questions in the array (question_1 is the first, question_2 is the second, etc.).

**Response:**

```json
{
  "success": true,
  "message": "Success",
  "survey": { /* Survey object */ },
  "questions": [ /* Array of saved question objects */ ]
}
```

### 2. Update Survey (PUT /api/surveys)

Updates an existing survey.  Replace `:id` with the actual survey ID.

**Request Body:**

```json
{
  "id": "survey_id_to_update", // Required
  "title": "Updated Survey Title", // Optional
  "type": "other", // Optional
  "questions": [
    {
      "id": "existing_question_id", // Required if updating an existing question
      "question": "Updated Question Text",
      "type": "text",
      "isSkip": true,
      "conditional_logic": {
        "condition": "equals",
        "expected_value": "4",
        "previous_question_id": "question_1" // Key referencing the question
      }
    },
    {
      "question": "New Question Text", // No 'id', so this will create a new question
      "type": "rating",
      "options": ["Yes", "No"]
    },
    // ... more questions
  ]
}
```

*   `id`: The ID of the survey to update. *This is required in the body, not the URL.*
*   `questions`:  If a question object has an `id`, it will be updated. If it *doesn't* have an `id`, it will be treated as a *new* question.

**Response:**

```json
{
  "success": true,
  "message": "Success",
  "survey": { /* Updated survey object */ },
  "questions": [ /* Array of updated/new question objects */ ]
}
```

### 3. Get Survey by ID (GET /api/survey)

Retrieves a specific survey by its ID.

**Request Body:**

```json
{
  "id": "survey_id_to_get" // Required
}
```

**Response:**

```json
{
  "success": true,
  "survey": { /* Survey object with populated addedBy and updatedBy */ },
  "questions": [ /* Array of question objects with populated addedBy and updatedBy */ ]
}
```

### 4. Get All Surveys by Author (GET /api/surveys)

Retrieves all surveys created by the authenticated user.

**Response:**

```json
{
  "success": true,
  "message": "Success",
  "data": [
    {
      "survey": { /* Survey object with populated addedBy and updatedBy */ },
      "questions": [ /* Array of question objects with populated addedBy and updatedBy */ ]
    },
    // ... more surveys
  ]
}
```

## Conditional Questions

The API supports conditional questions.  A question will only be displayed if the answer to a previous question meets a certain condition.

*   `conditional_logic`:  An object with the following properties:
    *   `condition`: The condition type (e.g., "equals", "not equals", "greater than", etc. - you'll need to implement the logic for these in your frontend).
    *   `expected_value`:  The value to compare against.
    *   `previous_question_id`: The key of the previous question (e.g., "question_1").

## Error Handling

The API returns JSON error responses with a `success: false` property and a `message` property describing the error.

## Example Usage (using `curl`):

**Create Survey:**

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer <your_jwt_token>" -d '{
  "title": "Test Survey",
  "type": "nps",
  "questions": [
    {
      "question": "How likely?",
      "type": "rating",
      "options": ["1", "2", "3", "4", "5"]
    }
  ]
}' /api/surveys
```

**Update Survey:**

```bash
curl -X PUT -H "Content-Type: application/json" -H "Authorization: Bearer <your_jwt_token>" -d '{
  "id": "the_survey_id",
  "title": "Updated Title"
}' /api/surveys/the_survey_id
```

**Get Survey:**

```bash
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer <your_jwt_token>" -d '{ "id": "the_survey_id" }' /api/survey
```

**Get All Surveys:**

```bash
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer <your_jwt_token>" /api/surveys
```

Remember to replace placeholders like `<your_jwt_token>` and `the_survey_id` with your actual values.  Use a tool like Postman or Insomnia for easier testing and inspection of responses.
```

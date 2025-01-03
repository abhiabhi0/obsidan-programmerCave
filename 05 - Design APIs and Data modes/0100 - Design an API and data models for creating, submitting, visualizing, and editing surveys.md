To design an API and data models for creating, submitting, visualizing, and editing surveys, it's essential to establish a structured approach that includes defining the core components of the survey application. Below is a comprehensive outline that covers the necessary aspects.

## API Design

### **Endpoints**

1. **Create Survey**
   - **POST /surveys**
     - **Request Body**: 
       ```json
       {
         "title": "string",
         "description": "string",
         "questions": [
           {
             "questionText": "string",
             "questionType": "multiple-choice | text | rating",
             "options": ["string"]
           }
         ]
       }
       ```
     - **Response**: Returns the created survey object.

2. **Submit Survey Response**
   - **POST /surveys/{surveyId}/responses**
     - **Request Body**:
       ```json
       {
         "respondentId": "string",
         "answers": [
           {
             "questionId": "string",
             "response": "string"
           }
         ]
       }
       ```
     - **Response**: Confirmation of submission.

3. **Get Survey**
   - **GET /surveys/{surveyId}**
     - **Response**: Returns the survey object including questions and options.

4. **Edit Survey**
   - **PUT /surveys/{surveyId}**
     - **Request Body**:
       ```json
       {
         "title": "string",
         "description": "string",
         "questions": [
           {
             "questionId": "string", // For existing questions
             "questionText": "string",
             "questionType": "multiple-choice | text | rating",
             "options": ["string"]
           }
         ]
       }
       ```
     - **Response**: Returns the updated survey object.

5. **Visualize Responses**
   - **GET /surveys/{surveyId}/responses**
     - **Response**: Returns aggregated data for visualization, such as average ratings or response counts.

### **Authentication**

Implement OAuth 2.0 for secure access to the API, ensuring that only authenticated users can create or edit surveys and submit responses.

## Data Models

### **Survey Model**

| Field          | Type                | Description                              |
|----------------|---------------------|------------------------------------------|
| id             | UUID                | Unique identifier for the survey        |
| title          | String              | Title of the survey                     |
| description    | String              | Description of the survey               |
| createdAt      | DateTime            | Timestamp when the survey was created   |
| updatedAt      | DateTime            | Timestamp when the survey was last edited |
| questions      | List<Question>      | List of questions in the survey         |
```sql
CREATE TABLE surveys (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```
### **Question Model**

| Field          | Type                | Description                              |
|----------------|---------------------|------------------------------------------|
| id             | UUID                | Unique identifier for the question      |
| surveyId       | UUID                | Reference to the parent survey           |
| questionText   | String              | The text of the question                 |
| questionType   | Enum                | Type of question (e.g., multiple-choice, text) |
| options        | List<String>        | Possible options for multiple-choice questions |
```sql
CREATE TABLE questions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    survey_id UUID REFERENCES surveys(id) ON DELETE CASCADE,
    question_text TEXT NOT NULL,
    question_type VARCHAR(50) CHECK (question_type IN ('multiple-choice', 'text', 'rating')),
    options TEXT[] -- Array of options for multiple-choice questions
);
```
### **Response Model**

| Field          | Type                | Description                              |
|----------------|---------------------|------------------------------------------|
| id             | UUID                | Unique identifier for the response      |
| surveyId       | UUID                | Reference to the related survey          |
| respondentId   | String              | Identifier for the respondent            |
| answers        | List<Answer>        | List of answers provided by respondent   |
```sql
CREATE TABLE responses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    survey_id UUID REFERENCES surveys(id) ON DELETE CASCADE,
    respondent_id VARCHAR(255) NOT NULL,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
### **Answer Model**

| Field          | Type                | Description                              |
|----------------|---------------------|------------------------------------------|
| questionId     | UUID                | Reference to the corresponding question  |
| response       | String              | The answer provided by the respondent    |
```sql
CREATE TABLE answers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    response_id UUID REFERENCES responses(id) ON DELETE CASCADE,
    question_id UUID REFERENCES questions(id) ON DELETE CASCADE,
    response TEXT NOT NULL
);
```

[[1001 - Why writing test case for handler function is complicated]]

Citations:
[1] https://developers.arcgis.com/survey123/guide/create-and-publish-surveys/
[2] https://www.qualtrics.com/support/employee-experience/employee-journey-analytics/creating-a-data-model/
[3] https://strapi.io/blog/creating-a-survey-application-using-strapi-and-react-js
[4] https://www.ijert.org/data-visualization-and-report-generation-tool-a-survey
[5] https://developer.blackbaud.com/lo-api/loapi/surveys/submitSurvey
[6] https://www.qualtrics.com/support/vocalize/mapping-cx-dashboard-data/data-modeler-cx/editing-a-data-model-cx/
[7] https://www.thoughtspot.com/data-trends/ai/ai-tools-for-data-visualization
[8] https://budibase.com/blog/data/how-to-create-a-data-model/
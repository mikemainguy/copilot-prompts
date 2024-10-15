# Generate Swagger/Openapi Docs
Collection of prompts to generate swagger docs from undocumented
apis.  Broken down by framework.

##Rails
```text
create a yaml file in openai3 format that describes the methods and signatures 
of all of the controllers in the the contollers/dashboard directory 
(include application, groups, labels, milestones, projects, snippets, and todos) 
assume hosts are https://api.example.com and https://staging.example.com 
that host the api. add detailed descriptions in the summary fields that explain 
edge cases and any important side effects. Include list of tables accessed by api 
endpoints in the description. Format list of tables like the following
 (tables => {table 1}, {table 2}). Don't include character ':' in summaries, titles,
  or descriptions
```
### Examples
#### Example Repo [https://github.com/gitlabhq/gitlabhq]()
Output
```openapi
openapi: 3.0.0
info:
  title: Dashboard API (Accesses various resources)
  version: 1.0.0
servers:
  - url: https://api.example.com
    description: Production server
  - url: https://staging.example.com
    description: Staging server
paths:
  /dashboard/application:
    get:
      summary: Get application details. Handles edge cases where the application is not found. Returns a 404 error if the application is not found (tables => {applications})
      operationId: getApplication
      responses:
        '200':
          description: Application details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Application'
        '404':
          description: Application not found
  /dashboard/groups:
    get:
      summary: List all groups. Handles edge cases where no groups are found. Returns an empty list if no groups are found (tables => {groups})
      operationId: listGroups
      responses:
        '200':
          description: A list of groups
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Group'
  /dashboard/labels:
    get:
      summary: List all labels with unique titles. Handles edge cases where no labels are found. Returns an empty list if no labels are found (tables => {labels})
      operationId: listLabels
      responses:
        '200':
          description: A list of labels
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Label'
  /dashboard/milestones:
    get:
      summary: List all milestones. Handles edge cases where no milestones are found. Returns an empty list if no milestones are found (tables => {milestones})
      operationId: listMilestones
      responses:
        '200':
          description: A list of milestones
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Milestone'
  /dashboard/projects:
    get:
      summary: List all projects. Handles edge cases where no projects are found. Returns an empty list if no projects are found (tables => {projects})
      operationId: listProjects
      responses:
        '200':
          description: A list of projects
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Project'
  /dashboard/snippets:
    get:
      summary: List all snippets for the current user with pagination support. Handles edge cases where the page parameter is missing or invalid. Returns an empty list if no snippets are found (tables => {snippets}, {users}, {projects})
      operationId: listSnippets
      parameters:
        - name: page
          in: query
          required: false
          schema:
            type: integer
          description: Page number for pagination
      responses:
        '200':
          description: A list of snippets
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Snippet'
  /dashboard/todos:
    get:
      summary: List all todos for the current user. Handles edge cases where no todos are found. Returns an empty list if no todos are found (tables => {todos}, {users})
      operationId: listTodos
      responses:
        '200':
          description: A list of todos
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Todo'
components:
  schemas:
    Application:
      type: object
      properties:
        id:
          type: string
          description: Application ID
        name:
          type: string
          description: Application name
    Group:
      type: object
      properties:
        id:
          type: string
          description: Group ID
        name:
          type: string
          description: Group name
    Label:
      type: object
      properties:
        id:
          type: string
          description: Label ID
        title:
          type: string
          description: Label title
        color:
          type: string
          description: Label color
    Milestone:
      type: object
      properties:
        id:
          type: string
          description: Milestone ID
        title:
          type: string
          description: Milestone title
    Project:
      type: object
      properties:
        id:
          type: string
          description: Project ID
        name:
          type: string
          description: Project name
    Snippet:
      type: object
      properties:
        id:
          type: string
          description: Snippet ID
        title:
          type: string
          description: Snippet title
        content:
          type: string
          description: Snippet content
        author:
          type: string
          description: Snippet author
        project:
          type: string
          description: Associated project
    Todo:
      type: object
      properties:
        id:
          type: string
          description: Todo ID
        title:
          type: string
          description: Todo title
        completed:
          type: boolean
          description: Whether the todo is completed

```

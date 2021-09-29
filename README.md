openapi: '3.0.0'
info:
  version: 1.0.0
  title: 'TestProject'
  license:
    name: MIT
servers:
  - url: http://localhost:5000
paths:
  /authentication/login:
    post:
      summary: login with email and password
      operationId: login
      responses:
        '200':
          description: successful login
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/LoginSuccess'
        '401':
          description: login error
  /registration/register:
    post:
      summary: register with email and password
      operationId: register
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/RegistrationData'
      responses:
        '200':
          description: response to registration request
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/ProfileResponse'
  /profile:
    get:
      summary: get current user
      security:
      - bearerAuth: []
      operationId: profile
      responses:
        '200':
          description: successful login
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: update profile
      security:
      - bearerAuth: []
      operationId: updateProfile
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/User'
      responses:
        '200':
          description: response to update profile request
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/ProfileResponse'
  /profile/change-password:
    post:
      summary: change password
      security:
      - bearerAuth: []
      operationId: changePassword
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/ChangePasswordData'
      responses:
        '200':
          description: response to update profile request
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/ChangePasswordResponse'
  /users:
    get:
      summary: get the list of users
      operationId: getUsers
      responses:
        '200':
          description: successful query
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: add a new element to the collection
      operationId: addUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/User'
      responses:
        '200':
          description: successful insertion
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
  /users/{userId}:
    get:
      summary: get a user by id
      operationId: getUser
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      responses:
        '200':
          description: successful query
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
    put:
      summary: update a user
      operationId: updateUser
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/User'
      responses:
        '200':
          description: successful update
    delete:
      summary: delete a user by id
      operationId: deleteUser
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      responses:
        '200':
          description: successful deletion
  /users/{userId}/pets:
    get:
      summary: get the list of pets that belong to the given user
      operationId: getPetsOfUser
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      responses:
        '200':
          description: successful query
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Pet'
    post:
      summary: add a new element to the collection
      operationId: addPetToUser
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/Pet'
      responses:
        '200':
          description: successful insertion
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Pet'
  /pets:
    get:
      summary: get the list of pets
      operationId: getPets
      responses:
        '200':
          description: successful query
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Pet'
    post:
      summary: add a new element to the collection
      operationId: addPet
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/Pet'
      responses:
        '200':
          description: successful insertion
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Pet'
  /pets/{petId}:
    get:
      summary: get a pet by id
      operationId: getPet
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      responses:
        '200':
          description: successful query
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Pet'
    put:
      summary: update a pet
      operationId: updatePet
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/Pet'
      responses:
        '200':
          description: successful update
    delete:
      summary: delete a pet by id
      operationId: deletePet
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
            format: int32
      responses:
        '200':
          description: successful deletion
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT 
  schemas:
    LoginSuccess:
      type: object
      required:
        - jwt
      properties:
        jwt:
          type: string
    RegistrationData:
      type: object
      required:
        - userName
        - email
        - password
      properties:
        userName:
          type: string
        email:
          type: string
        password:
          type: string
    ProfileResponse:
      type: object
      required:
        - success
        - isUserNameInUse
        - isEmailInUse
      properties:
        success:
          type: boolean
        isUserNameInUse:
          type: boolean
        isEmailInUse:
          type: boolean
    ChangePasswordData:
      type: object
      required:
        - oldPassword
        - newPassword
      properties:
        oldPassword:
          type: string
        newPassword:
          type: string
    ChangePasswordResponse:
      type: object
      required:
        - success
      properties:
        success:
          type: boolean
    User:
      type: object
      required:
        - id
        - user_name
        - email
      properties:
        id:
          type: integer
          format: int32
          readOnly: true
        user_name:
          type: string
        email:
          type: string
    Pet:
      type: object
      required:
        - id
        - name
        - species
        - user_id
      properties:
        id:
          type: integer
          format: int32
          readOnly: true
        name:
          type: string
        species:
          type: string
        user_id:
          type: integer
          format: int32

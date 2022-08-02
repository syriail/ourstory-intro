# OUR STORY

Our Story aka. Ourstory project is a **multi-lingual, flexible and expandable** archive of collections of stories. Visitors can search this archive for stories. The archive does not only support simple text search, but rahter semantic search.

## Story

A story is a material collected by researchers and mostly told by someone. A story could be oral history, folklore songs, fairy tale ..etc

Stories could be backed with video, audio or image media such as a film recording the story teller while telling the story.

## Collection

Stories are collected in a context of projects carried out by organizations interested in heritage and oral history of people.

All stories collected in a context of a project are organized in collection. Hakawati Collection is an example of such project in which more than 400 stories were collected.

# Features

The archive has the following main features:

- **Expandable:** Collections and stories could be added dynamically.
- **Flexible:** Collection-specific fields can be defined dynamically, so these fields appear only in stories which belong to this collection.
- **Multi-lingual:** A new language could be added with a minimum technical efforts.
- **Semantically Searchable:** Archive supports semantic search according to each language's characterstics such as plural recognition in Arabic.
- **Role-based Functionalities:** Users can perform actions based on their role in the system and their role in each collection.

# System Parts

The system consists of three **_separate_** parts (sub-projects)

## Backend

The server-side components and services which provide the core funcionalities and logic of the system.
The backend is being developed using Serverless and Event Driven architectures.

[More details](https://github.com/syriail/ourstory-backend)

## Manager

A web application where authorized users can define collections, add stories, and translate stories

The manager is being developed using ReactJs

[More details](https://github.com/syriail/ourstory-manager)

## Frontend

Where visitors search for stories and get the results accordingly.

The manager is being developed using ReactJs

[More details](https://github.com/syriail/ourstory-frontend)

# APIs

## Create Collection

- REST API: POST /collections

- Athorized Roles: Collection Manger

- Body Params:

  - defaultLocale: The default language of the collection
  - name: Collection's name
  - description?: Collection's description
  - managerId: Manager's id
  - editors?: List of editors' ids `editors:[string]`
  - tags?: List of slugs of collection-specific fields `tags:[{slug, name}]`

- Status Code:

  - 201: Success
  - 403: Unauthorized
  - 400: Missing param

- Returns: The newely added collectoin `{collection: {}}`
  - id: Collection id
  - name: Collection's name in user's language if available, otherwise in default language
  - description?: Collection's description
  - defaultLocale: The default language in which the collection was originally created
  - availableTranslations: []
  - manager: Collection manager object
  - createdAt: Create date
  - Editors?: list of authorized editors
  - tags?: list of tags

## Update Collection

- REST API: PATCH /collections

- Authorized Roles: Collection Manager

- Body Params:

  - id: Collection id
  - defaultLocale: The default language of the collection
  - name: Collection's name
  - description?: Collection's description
  - managerId: Manager's id
  - editors?: List of editors' ids `editors:[string]`
  - tags?: List of slugs of collection-specific fields `tags:[{slug, name}]`

- Status Code:

  - 201: Success
  - 403: Unauthorized
  - 400: Missing param

- Return: Nothing

## Get Collections

It fetches all the collection which the user is the manager of or editor

- REST API: GET /collections?locale={locale}

- Authorized Roles: Collection Manager, Collection Editor

- Query Params:

  - locale: The user locale to fetch information accordingly

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing locale

- Returns: List of collections `{ collections: []}`
  - id: Collection id
  - name: Collection's name in the language specified in the query if available, otherwise in default language
  - description: Collection's description in the language specified in the query if available, otherwise in default language
  - defaultLocale: The default language in which the collection was originally created
  - availableTranslations: The languages the collection is translated into execluding the default locale. Expample `['ar', 'de']`
  - manager: Collection manager object
  - createdAt: Create date
  - Editors: list of authorized editors

## Get Collection

- REST API: GET /collections/details/{collectionId}?locale={locale}

- Authorized Roles: Collection Manager, Collection Editor

- Params:

  - collectionId: Collection's id
  - locale: The language in which the data need to be presented

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing collectionId or locale

- Returns: Collection's details `{collection:{}}`
  - id: Collection id
  - name: Collection's name in the language specified in the query if available, otherwise in default language
  - description?: Collection's description in in the language specified in the query if available, otherwise in default language
  - defaultLocale: The default language in which the collection was originally created
  - availableTranslations: The languages the collection is translated into execluding the default locale. Expample `['ar', 'de']`
  - manager: Collection manager object
  - createdAt: Create date
  - Editors?: list of authorized editors
  - tags?: Collection's tags `tags:[{slug, name}]`

## Translate Collection

Translate a collection into a given language

- REST API: POST /collections/translate

- Body:

  - id: Collection id
  - locale: The language which the colllection must translated into
  - name: The translated name
  - description?: The translated description
  - tags?: The translted tags `tags:[{slug, name}]`

- Status Code:

  - 201: Success
  - 403: Unauthorized
  - 400: Missing params

- Returns: Nothing

## Create Story

Add a story to a collection in Collection's default language

- REST API: POST /stories

- Authorized Roles: Collection Manager, Collection Editor

- Body Params:

  - collectionId: Collection id
  - defaultLocale: The default language in which the collection was originally created
  - storyType: The type of the story, picked from pre-defined list
  - storyTitle: Story's title
  - storyTellerAge?: The age of story teller
  - storyTellerGender?: The gender of the story teller
  - storyAbstraction?: The abstraction of the story
  - storyTranscript?: The transcript of the story
  - storyTellerName?: The name of the story teller
  - storyTellerPlaceOfOrigin?: Place of origin of the story teller
  - storytellerResidency?: The residency of the story teller
  - storyCollectorName?: The name of the person who collected the story
  - tags?: Tags values for this story `tags:[{slug, tagValue}]`

- Status Code:

  - 201: Success
  - 403: Unauthorized
  - 400: Missing required params

- Returns: The newly created story `{story:{}}`
  - id: Story id
  - collectionId: Collection id
  - defaultLocale: The default language in which the collection was originally created
  - storyType: The type of the story
  - storyTitle: Story's title
  - storyTellerAge?: The age of story teller
  - storyTellerGender?: The gender of the story teller
  - storyTitle: Story title
  - storyAbstraction?: The abstraction of the story
  - storyTranscript?: The transcript of the story
  - storyTellerName?: The name of the story teller
  - storyTellerPlaceOfOrigin?: Place of origin of the story teller
  - storytellerResidency?: The residency of the story teller
  - storyCollectorName?: The name of the person who collected the story

## Update Stroy

- REST API: PATCH /story/{storyId}

- Authorized Roles: Collection Manager, Collection Editor

- The body and return response is identical to Create Story

- Status Code:

  - 201: Success
  - 403: Unauthorized
  - 400: Missing required params

## Delete Story

Delete the given story with all available translations

- REST API: DELETE /story/{storyId}

- Authorized Roles: Collection Manager, Collection Editor

- Status Code:

  - 204: Success
  - 403: Unauthorized

## Get Story Details

- REST API: GET /stories/details/{storyId}?locale={locale}

- Authorized Roles: Collection Manager, Collection Editor

- Params:

  - storyId: Story id,
  - locale: The language in which the data need to be presented

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing storyId or locale

- Returns: List of stories `{}`
  - id: Story id
  - collectionId: Collection's id
  - defaultLocale: The default language in which the collection was originally created
  - storyType: The type of the story
  - storyTitle: Story's title in the given language if available, otherwise in default language
  - storyTellerAge?: The age of story teller
  - storyTellerGender?: The gender of the story teller
  - tags?: Tags values for this story `tags:[{storyId, collectionId, locale, slug, name, value}]`
  - title: Story title
  - storyAbstraction?: The abstraction of the story
  - storyTranscript?: The transcript of the story
  - storyTellerName?: The name of the story teller
  - storyTellerPlaceOfOrigin?: Place of origin of the story teller
  - storytellerResidency?: The residency of the story teller
  - storyCollectorName?: The name of the person who collected the story
  - mediaFiles?: List of available media `mediaFiles:[format, mediaPath]` format is one of VIDEO, AUDIO or IMAGE

## Get Upload Signed Url

Gets a signed url to upload a media file to S3 media bucket

- REST API: GET /uploadUrl/{storyId}

- Authorized Roles: Collection Manager, Collection Editor

- Params:

  - storyId: Story id

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing storyId

- Returns:
  - uploadUrl: Signed url to upload the file to
  - attachmentUrl: The path which the story should store to refer to the media

## Get Download Signed Url

Gets a signed url to download/view a media file

- REST API: GET /downloadUrl?path={path}

- Params:

  - path: media path

- Authorized Roles: Authorized/Unauthorized users

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing path

- Returns the url as string to view/download the media

## Get Tags

Get all tags with there names

- REST API: GET /tags?locale={locale}

- Params:

  - locale: The locale in which the tags' names should be returned

- Status Code:

  - 200: Success
  - 403: Unauthorized
  - 400: Missing locale

- Returns: List of tags `{tags:[]}`
  - slug: Slug of the tag
  - name: The name of the tag in the language specified.

# Data Model

```
export enum MediaFormat {
  AUDIO = 'AUDIO',
  VIDEO = 'VIDEO',
  IMAGE = 'IMAGE',
}
export enum Gender {
  MALE = 'MALE',
  FEMALE = 'FEMALE',
  UNSPECIFIED = 'UNSPECIFIED'
}
export enum StoryType {
  MYTHS = 'MYTHS',
  FABLE = 'FABLE',
  FOLK_TALE = 'FOLK_TALE',
  WONDER_TALE = 'WONDER_TALE',
  WISDOM_TALE = 'WISDOM_TALE',
  ALLEGORY = 'ALLEGORY',
  TALL_TALE = 'TALL_TALE',
  RELIGIOUS_TALE = 'RELIGIOUS_TALE',
  PROVERBS = 'PROVERBS',
  PERSONAL_EXPERIENCE = 'PERSONAL_EXPERIENCE',
  EPIC = 'EPIC',
  FOLK_SONG = 'FOLK_SONG',
}
export enum EmployeeRole {
  ADMIN = 'ADMIN',
  COLLECTION_MANAGER = 'COLLECTION_MANAGER',
  EDITOR = 'EDITOR'
}
export enum TranslableType{
  COLLECTION = 'COLLECTION',
  STORY = 'STORY',
  TAG = 'TAG'
}
export interface MediaFile {
  format: MediaFormat,
  mediaPath: string
}

export interface Tag{
  slug: string,
  name: string
}
export interface Employee{
  id: string,
  firstName?: string,
  lastName?: string,
  locale: string,
  roles: EmployeeRole[]
}
export interface Collection{
  id: string
  name: string
  description?: string
  createdAt: string
  manager: Employee
  availableTranslations: string[]
  defaultLocale: string
  editors?: Employee[]
  tags?: Tag[]
}
export interface Story{
  id: string
  collectionId: string
  defaultLocale: string
  availableTranslations: string[]
  storyTitle: string
  storyAbstraction?: string
  storyTranscript?: string
  storyTellerName?: string
  storyTellerPlaceOfOrigin?: string
  storyTellerResidency?: string
  storyCollectorName?: string
  mediaFiles?: MediaFile[]
  storyType: string[]
  storyTellerAge?: number
  storyTellerGender?: string
  tags?: TagValue[]
}
export interface TagValue{
  storyId: string
  collectionId: string
  locale: string
  slug: string
  name: string
  value: string
}

export interface UploadAttachmentData{
uploadUrl: string
attachmentUrl: string
}

export const languages = ['en', 'ar']

```

# Add New Language

Ourstory is a mulilinugal stories archive. English and Arabic languages are already set up, configured and tuned.

Adding a new language is easy by following these steps:

- Algolia
  - Create new indexes, one index for each stage/language.
    The name of the index should be `{stage}_ourstory_{locale}`
    This is not regid requirement, so it could be changed also.
  - Configure the non-production index by copying searchable attributes and facets from the existing indexes.
  - Tune the non-production index according to the language characteristics.
    For example, Arabic language has its own search issures which needs to be address by adding more configurations.
  - Test the index
  - If the search results are satisfying, add the production index
- UI
  - In both ourstory-manager and ourstory-frontend add a new translation for UI in public/locales/
- Model: Add the new language code to the languages array

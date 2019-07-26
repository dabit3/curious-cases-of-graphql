# Curious Cases of GraphQL

![](header.png)

## Case 1: Real-time collaborative drawing canvas with GraphQL & AWS AppSync

[GitHub repo](https://github.com/dabit3/appsync-graphql-real-time-canvas)

Base schema:

```graphql
type Canvas @model {
  id: ID!
  clientId: String!
  data: String!
}

type Mutation {
  createCanvas(input: CreateCanvasInput!): Canvas
  updateCanvas(input: UpdateCanvasInput!): Canvas
  deleteCanvas(input: DeleteCanvasInput!): Canvas
}

type Query {
  getCanvas(id: ID!): Canvas
  listCanvass(filter: ModelCanvasFilterInput, limit: Int, nextToken: String): ModelCanvasConnection
}


type Subscription {
  onCreateCanvas: Canvas @aws_subscribe(mutations: ["createCanvas"])
  onUpdateCanvas: Canvas @aws_subscribe(mutations: ["updateCanvas"])
}
```

## Case 2: GraphQL Image Rekognition

[GitHub repo](https://github.com/dabit3/appsync-image-rekognition)

Base schema

```graphql
type ImageData {
  data: String!
}

type Query {
  fetchImage(imageInfo: String!): ImageData
}
```

## Case 3: SpeakerChat - Real-time event comment platform with markdown support

[GitHub repo](https://github.com/dabit3/speakerchat)

Base schema:

```graphql
type Talk @model {
  id: ID!
  title: String!
  speakerName: String!
  clientId: ID
  speakerImage: String
  comments: [Comment] @connection(name: "TalkComments")
}

type Comment @model {
  id: ID!
  talkId: ID!
  clientId: ID
  talk: Talk @connection(sortField: "createdAt", name: "TalkComments", keyField: "talkId")
  text: String
  createdAt: String
  createdBy: String
}

type ModelCommentConnection {
  items: [Comment]
  nextToken: String
}

type Subscription {
  onCreateCommentWithId(talkId: ID!): Comment
		@aws_subscribe(mutations: ["createComment"])
}

type Query {
  listCommentsForTalk(talkId: ID!): ModelCommentConnection
}
```

## Case 4: Hype Beats

[GitHub repo](https://github.com/dabit3/hype-beats)

Base schema:

```graphql
type DrumMachine @model {
  id: ID!
  clientId: ID!
  beats: String!
  name: String!
}

type Query {
  getDrumMachine(id: ID!): DrumMachine
  listDrumMachines(filter: ModelDrumMachineFilterInput, limit: Int, nextToken: String): ModelDrumMachineConnection
}

type Mutation {
  createDrumMachine(input: CreateDrumMachineInput!): DrumMachine
  updateDrumMachine(input: UpdateDrumMachineInput!): DrumMachine
  deleteDrumMachine(input: DeleteDrumMachineInput!): DrumMachine
}

type Subscription {
  onCreateDrumMachine: DrumMachine @aws_subscribe(mutations: ["createDrumMachine"])
  onUpdateDrumMachine: DrumMachine @aws_subscribe(mutations: ["updateDrumMachine"])
}

```

## Case 5: GraphQL text to audio translation

[GitHub repo](https://github.com/dabit3/appsync-web-translator)

Base schema:

```graphql
type Query {
  getTranslatedSentence(sentence: String!, code: String!): TranslatedSentence
}

type TranslatedSentence {
  sentence: String!
}
```

## Case 6: Infra as code with Conference app in a box

[GitHub repo](https://github.com/dabit3/conference-app-in-a-box)

Base schema:

```graphql
type Talk @model {
  id: ID!
  name: String!
  speakerName: String!
  speakerBio: String!
  time: String
  timeStamp: String
  date: String
  location: String
  summary: String!
  twitter: String
  github: String
  speakerAvatar: String
  comments: [Comment] @connection(name: "TalkComments")
}

type Comment @model {
  id: ID!
  talkId: ID
  talk: Talk @connection(sortField: "createdAt", name: "TalkComments", keyField: "talkId")
  message: String
  createdAt: String
  createdBy: String
  deviceId: ID
}

type Report @model {
	id: ID!
	commentId: ID!
	comment: String!
	talkTitle: String!
	deviceId: ID
}

type ModelCommentConnection {
	items: [Comment]
	nextToken: String
}

type Query {
  listCommentsByTalkId(talkId: ID!): ModelCommentConnection
}

type Subscription {
  onCreateCommentWithId(talkId: ID!): Comment
		@aws_subscribe(mutations: ["createComment"])
}
```

## Case 7 - GraphQL SMS in Markdown

[GitHub repo](https://github.com/dabit3/sms-graphql-ui/settings)

Base schema:

```graphql
type SMS {
	originationNumber: String!
	messageBody: String!
  id: ID!
}

type Query {
	getSMS(originationNumber: String!): SMS
	listSMS(filter: TableSMSFilterInput, limit: Int, nextToken: String): SMSConnection
}

type Subscription {
	onCreateSMS(originationNumber: String, messageBody: String): SMS
		@aws_subscribe(mutations: ["createSMS"])
	onUpdateSMS(originationNumber: String, messageBody: String): SMS
		@aws_subscribe(mutations: ["updateSMS"])
	onDeleteSMS(originationNumber: String, messageBody: String): SMS
		@aws_subscribe(mutations: ["deleteSMS"])
}

type Mutation {
	createSMS(input: CreateSMSInput!): SMS
	updateSMS(input: UpdateSMSInput!): SMS
	deleteSMS(input: DeleteSMSInput!): SMS
}
```
# @simplifycms/client

A lightweight TypeScript client library for programmatically interacting with the YourCMS API. This package allows developers to define schemas and seed initial content for their CMS projects directly in code, making it ideal for setting up projects as part of a development workflow. For ongoing content management, use the YourCMS Studio UI.

## Features

- **Schema Definition**: Define and migrate content schemas (e.g., fields like `title`, `body`) to your CMS project.
- **Content Seeding**: Create initial content programmatically to get your project started.
- **API Integration**: Works seamlessly with the YourCMS API using credentials generated by the `cms-cli`.
- **Query Language**: Query and mutate data with a simple, schema-driven API (e.g., `find`, `create`).

This package is designed for developers who want to version-control their schemas and automate initial setup, leaving further content management to the Studio UI.

## Installation

Install the package via npm:

```bash
npm install @simplifycms/client
```

You’ll need:

A YourCMS project set up via the cms-cli (run cms-cli init to authenticate and create a project).
The .env file generated by the CLI, containing:
- CMS_API_URL (e.g., https://api.your-cms.com)
- CMS_API_KEY (your authentication token)
- CMS_PROJECT_ID (your project’s ID)

## Usage

1. Initialize the Client
Create an instance of CMSClient with your project credentials:

```ts
import { CMSClient } from '@simplifycms/client';
import * as dotenv from 'dotenv';

dotenv.config(); // Load .env file

const client = new CMSClient({
  apiUrl: process.env.CMS_API_URL!,
  apiKey: process.env.CMS_API_KEY!,
  projectId: process.env.CMS_PROJECT_ID!,
});
```
2. Define and Migrate a Schema
Define your schema in code and migrate it to the CMS:

```ts
import { CMSClient, Schema } from '@your-cms/client';

const client = new CMSClient({
  apiUrl: process.env.CMS_API_URL!,
  apiKey: process.env.CMS_API_KEY!,
  projectId: process.env.CMS_PROJECT_ID!,
});

const blogSchema: Schema = {
  fields: [
    { name: 'title', type: 'string', required: true },
    { name: 'body', type: 'text' },
    { name: 'published', type: 'boolean', default: false },
    { name: 'publishDate', type: 'date' },
  ],
};

async function migrateSchema() {
  try {
    await client.migrateSchema(blogSchema);
    console.log('Schema set up successfully!');
  } catch (error) {
    console.error('Schema migration failed:', error);
  }
}

migrateSchema();
```

3. Seed Initial Content
Define and create initial content based on your schema:

```ts
import { CMSClient } from '@your-cms/client';

const client = new CMSClient({
  apiUrl: process.env.CMS_API_URL!,
  apiKey: process.env.CMS_API_KEY!,
  projectId: process.env.CMS_PROJECT_ID!,
});

const initialPost = {
  title: 'Welcome to My Blog',
  body: 'This is the first post in our CMS-powered blog!',
  published: true,
  publishDate: new Date().toISOString(),
};

async function seedContent() {
  try {
    const createdPost = await client.createContent(initialPost);
    console.log('Initial content created! Manage more in the Studio UI.');
  } catch (error) {
    console.error('Content creation failed:', error);
  }
}

seedContent();
```

4. Query and Mutate Data
Use the query language to fetch and create content based on your schema:

```ts
import { CMSClient } from '@simplifycms/client';

const client = new CMSClient({
  apiUrl: process.env.CMS_API_URL!,
  apiKey: process.env.CMS_API_KEY!,
  projectId: process.env.CMS_PROJECT_ID!,
});

async function manageContent() {
  try {
    // Query posts
    const posts = await client
      .collection('posts')
      .find({
        where: { published: true },
        select: ['title', 'body'],
        limit: 5,
      });
    console.log('Published posts:', posts);

    // Create a post
    const newPost = await client
      .collection('posts')
      .create({
        title: 'Welcome to My Blog',
        body: 'This is the first post in our CMS-powered blog!',
        published: true,
        publishDate: new Date().toISOString(),
      });
    console.log('Created post:', newPost);
  } catch (error) {
    console.error('Content operation failed:', error);
  }
}

manageContent();
```

```ts
  Methods
    find(options?: QueryOptions): Promise<ContentItem[]>
  Queries content items from the collection.
    options:
    where: Filter conditions (e.g., { published: true }).
    select: Fields to return (e.g., ['title', 'body']).
    limit: Max number of items (e.g., 5).
    offset: Pagination offset (e.g., 10).
    sort: Sorting (e.g., { field: 'title', order: 'asc' }).
  Returns an array of content items.
    create(content: Record<string, any>): Promise<ContentItem>
  Creates a content item in the collection.
    content: An object with key-value pairs matching the schema fields.
  Returns the created content item with an id.
  Types
    Schema: { fields: SchemaField[] }
    SchemaField: { name: string, type: 'string' | 'text' | 'number' | 'boolean' | 'date', required?: boolean, default?: any }
    ContentItem: { id: string, [key: string]: any }
    QueryOptions: { where?: Record<string, any>, select?: string[], limit?: number, offset?: number, sort?: { field: string, order: 'asc' | 'desc' } }
```

## API Reference
`CMSClient`
Constructor

```ts
new CMSClient(config: ClientConfig)
```

- `config.apiUrl`: The base URL of your CMS API (e.g., https://api.your-cms.com).
- `config.apiKey`: Your authentication token from the CLI.
- `config.projectId`: The ID of your CMS project.

### Methods
  migrateSchema(schema: Schema): Promise<void>
  Migrates a schema to your CMS project.
  schema: An object with fields defining the structure.
  createContent(content: ContentItem): Promise<ContentItem>
  Creates a content item based on the schema.
  content: An object with key-value pairs matching the schema fields.
  Returns the created content item with an id.

### Types
  Schema: { fields: SchemaField[] }
  SchemaField: { name: string, type: 'string' | 'text' | 'number' | 'boolean' | 'date', required?: boolean, default?: any }
  ContentItem: { [key: string]: any }

## Studio UI
For managing content (e.g., editing, deleting, or viewing), log into the YourCMS Studio UI at https://your-cms.com/studio (or your custom URL) using the same credentials. This package is for initial setup; the Studio handles everything else!

## Requirements
Node.js 14+ (for ES modules compatibility).
A YourCMS account and project (set up via cms-cli).

Contributing
Feel free to submit issues or PRs to the [https://github.com/oussamachah2020/simplifycms](Repo)

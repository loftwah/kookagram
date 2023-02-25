# KookaGram

![KookaGram](https://user-images.githubusercontent.com/19922556/221359849-2b0b06ca-4f5b-4117-9ed2-6ee981d065a2.jpg)

KookaGram is a self-hosted and open-source social media management application designed to help users manage and schedule their social media posts. With KookaGram, users can easily schedule and manage their posts, view analytics, and bill users through the Stripe API. The application allows users to post to Facebook, LinkedIn, Twitter, Instagram, and TikTok, and it includes a queue for scheduled posts, a section for previous posts and post management, and a scheduler for bulk content creation scheduled in the future. The application also supports importing from CSV, XLS, and common file formats and posting automatically from RSS feeds. KookaGram is built using Appwrite for the backend and Flutter for the frontend and is named after the iconic Australian bird, the Kookaburra.

## Features

* Configure social media accounts locally or through the UI
* Two-factor authentication with email or Google Authenticator
* Post to Facebook, LinkedIn, Twitter, Instagram, and TikTok
* CLI and web UI for publishing to social media pages
* Queue for scheduled posts and post management
* Section for previous posts and post management (removal or reposting)
* Draft version of posts that won't be published until set to do so
* Scheduler for bulk content creation scheduled in the future
* Import from CSV, XLS, and common file formats
* Posting automatically from RSS feeds and managing which accounts to use
* Calendar view and modification of the schedule
* Analytics for all posts including optimal times and best posts

## Billing

Billing will be handled through the Stripe API. Users can sign up for a subscription plan and be billed monthly. The subscription plan will include a set number of social media accounts, scheduled posts, and analytics features. Additional features can be purchased as add-ons.

## Design

The design of the application will be simple and intuitive. The application will be built using the Flutter framework for the frontend and Appwrite for the backend. The application will consist of two components: a CLI for publishing and scheduling posts from the command line and a web UI for managing social media accounts, scheduling posts, and viewing analytics.

### Backend

The backend will be built using Appwrite, a secure backend as a service platform. Appwrite provides built-in user authentication and account management, database storage, and file storage.

The backend will consist of several components:

* User authentication and account management
* Social media account management
* Post scheduling and management
* Analytics tracking and reporting
* Stripe integration for billing and subscription management

### Frontend

The frontend will be built using Flutter, a mobile and web application development framework. The frontend will consist of two components:

* CLI for publishing and scheduling posts from the command line
* Web UI for managing social media accounts, scheduling posts, and viewing analytics

### Database

The application will use a NoSQL database for storing user data, social media accounts, and scheduled posts. The database will be hosted on the Appwrite platform.

### Deployment

The application can be deployed to a cloud platform such as AWS, Google Cloud, or DigitalOcean. It can also be deployed on a self-hosted server or a VPS.

## REST API

KookaGram's REST API allows you to access the backend through HTTP requests without the need for an SDK. Each endpoint in the API represents a specific operation on a specific resource. The REST API is authenticated with API keys instead of account sessions, and JWT authentication is used for server applications to act on behalf of a user.

## Query Methods

Appwrite's SDKs provide a Query class to generate query strings. When using Appwrite without an SDK, you can template your own strings with the format below.

Query strings are passed to Appwrite using the queries parameter. You can attach multiple query strings by including the array parameter multiple times in the query string: queries\[\]="..."&queries\[\]="..."

| Query Method | Query String |
| --- | --- |
| equal | `equal("attribute", [value])` |
| notEqual | `notEqual("attribute", [value])` |
| lessThan | `lessThan("attribute", [value])` |
| lessThanEqual | `lessThanEqual("attribute", [value])` |
| greaterThan | `greaterThan("attribute", [value])` |
| greaterThanEqual | `greaterThanEqual("attribute", [value])` |
| search | `search("attribute", [value1])` |
| orderDesc | `orderDesc("attribute")` |
| orderAsc | `orderAsc("attribute")` |
| cursorAfter | `cursorAfter("documentId")` |
| cursorBefore | `cursorBefore("documentId")` |
| limit | `limit(0)` |
| offset | `offset(0)` |

Best Practice When using greater than, greater than or equal to, less than, or less than or equal to, it is not recommended to pass in multiple values. While the API will accept multiple values and return results with or logic, it's best practice to pass in only one value for performance reasons.

OpenAPI and Swagger Specs Appwrite provides a full REST API specification in the OpenAPI 3 and Swagger 2 formats every release. These can be accessed through Appwrite's GitHub repository and rendered using a variety of parsers and tools.

Find the REST API specification for your Appwrite version

## Contributing

We welcome contributions to KookaGram! To contribute to the project, please follow these steps:

1. Fork the repository and clone it to your local machine.
2. Create a new branch for your feature or bug fix.
3. Write your code and commit your changes. Be sure to include tests for your code.
4. Push your branch to your fork and submit a pull request.
5. One of the maintainers will review your pull request and provide feedback.

Please note that all contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## License

This project is licensed under the BSD-3 License - see the [LICENSE](LICENSE) file for details.

# Getting Started with KookaGram

Building KookaGram requires knowledge of several technologies, including Appwrite, Flutter, and Stripe API, so it is recommended that you have some experience with these technologies before starting.

## Prerequisites

Before you begin building KookaGram, you need to have the following tools installed on your machine:

* Flutter SDK ([https://flutter.dev/docs/get-started/install](https://flutter.dev/docs/get-started/install))
* Dart SDK ([https://dart.dev/get-dart](https://dart.dev/get-dart))
* Git ([https://git-scm.com/downloads](https://git-scm.com/downloads))
* Appwrite server ([https://appwrite.io/docs/installation](https://appwrite.io/docs/installation))
* Stripe account ([https://stripe.com/docs/getting-started](https://stripe.com/docs/getting-started))

Once you have installed these tools, you can proceed with building KookaGram.

## Step 1: Create a new Flutter project

To create a new Flutter project, run the following command in your terminal:

`flutter create kookagram`

This will create a new Flutter project named "kookagram" in your current directory.

## Step 2: Add Appwrite and Stripe dependencies

Add the Appwrite and Stripe dependencies to your project by adding the following lines to your `pubspec.yaml` file:

```yaml
dependencies:
  appwrite: ^0.8.3
  stripe_payment: ^1.0.6
```

## Step 3: Configure Appwrite

To configure Appwrite, you need to set up an instance of the Appwrite server on your local machine or in the cloud. You can follow the instructions in the Appwrite documentation to install the server.

Once you have installed the server, you need to create a new project in the Appwrite console and set up the following collections:

* Users
* Social Media Accounts
* Scheduled Posts

You can set up these collections by navigating to the "Database" section of your project in the Appwrite console and clicking the "Add Collection" button.

## Step 4: Configure Stripe

To configure Stripe, you need to create a new account on the Stripe website and obtain an API key. You can follow the instructions in the Stripe documentation to set up your account.

Once you have obtained your API key, you need to add it to your Flutter project by creating a new file named `.env` in the root directory of your project and adding the following line:

`STRIPE_API_KEY=<your_api_key>`

You can then load the API key in your Dart code using the `dotenv` package.

## Step 5: Build the backend

To build the backend of KookaGram, you need to create a new Appwrite client and configure it with your Appwrite server credentials. You can do this in your `main.dart` file by adding the following code:

```dart
import 'package:appwrite/appwrite.dart';

final client = Client()
    .setEndpoint('https://[HOSTNAME_OR_IP]/v1') // Replace with your Appwrite server hostname or IP address
    .setProject('<project_id>') // Replace with your Appwrite project ID
    .setKey('<secret_key>'); // Replace with your Appwrite project secret key
```

You can then create a new instance of the `UserService` class and use it to handle user authentication and account management:

```dart
final userService = UserService(client);

// Create a new user
await userService.create(email: 'user@example.com', password: 'password');

// Authenticate a user
final session = await userService.createSession(email: 'user@example.com', password: 'password');

// Get the user's account information
final account = await userService.get();
```

You can also create instances of the `DatabaseService` class to handle database operations:

```dart
final database = Database(client);

// Create a new document in the Social Media Accounts collection
await database.createDocument(
    collectionId: 'socialMediaAccounts',
    data: {
        'userId': 'user_id',
        'type': 'facebook',
        'accessToken': 'access_token',
    }
);

// Get all documents in the Scheduled Posts collection
final result = await database.listDocuments(
    collectionId: 'scheduledPosts',
    filters: [
        Filter.json('{"userId":"user_id"}')
    ]
);
final documents = result.data['documents'];
```

You can also use the `FileService` class to handle file uploads and the `StorageService` class to handle file storage.

## Step 6: Build the frontend

To build the frontend of KookaGram, you need to create a new Flutter screen for each of the application's features. You can use the Flutter framework's built-in widgets to create the user interface.

For example, to create a screen for adding a social media account, you can create a new Dart file named `add_account_screen.dart` and add the following code:

```dart
import 'package:flutter/material.dart';
import 'package:appwrite/appwrite.dart';

class AddAccountScreen extends StatefulWidget {
  const AddAccountScreen({Key? key}) : super(key: key);

  @override
  _AddAccountScreenState createState() => _AddAccountScreenState();
}

class _AddAccountScreenState extends State<AddAccountScreen> {
  final _formKey = GlobalKey<FormState>();
  final _client = Client()
      .setEndpoint('https://[HOSTNAME_OR_IP]/v1')
      .setProject('<project_id>')
      .setKey('<secret_key>');
  final _database = Database(_client);
  final _userService = UserService(_client);
  final _socialMediaAccountsCollectionId = 'socialMediaAccounts';

  String _type = '';
  String _accessToken = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Add Social Media Account'),
      ),
      body: Form(
        key: _formKey,
        child: Column(
          children: [
            TextFormField(
              decoration: InputDecoration(
                labelText: 'Type (e.g. Facebook, LinkedIn, Twitter)',
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter a social media type';
                }
                return null;
              },
              onSaved: (value) {
                _type = value!;
              },
            ),
            TextFormField(
              decoration: InputDecoration(
                labelText: 'Access Token',
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter an access token';
                }
                return null;
              },
              onSaved: (value) {
                _accessToken = value!;
              },
            ),
            ElevatedButton(
              onPressed: _submit,
              child: Text('Add Account'),
            ),
          ],
        ),
      ),
    );
  }

  void _submit() async {
    if (_formKey.currentState!.validate()) {
      _formKey.currentState!.save();

      final user = await _userService.get();
      final userId = user['\$id'];

      await _database.createDocument(
        collectionId: _socialMediaAccountsCollectionId,
        data: {
          'userId': userId,
          'type': _type,
          'accessToken': _accessToken,
        },
      );

      Navigator.pop(context);
    }
  }
}
```

You can then use the `Navigator` class to navigate to this screen when the user clicks a button:

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('KookaGram'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => AddAccountScreen()),
                );
              },
              child: Text('Add Social Media Account'),
            ),
          ],
        ),
      ),
    );
  }
}
```

You can repeat this process for each of the application's features, such as scheduling posts, viewing analytics, and managing subscriptions.

## Step 7: Deploy the application

To deploy the application, you can use a cloud platform such as AWS, Google Cloud, or DigitalOcean. You can follow the platform's documentation to set up a new server instance and deploy your application to it.

Here are some general steps to deploy a Flutter app on a cloud platform:

1. Build the Flutter app for web using the `flutter build web` command.
2. Copy the contents of the `build/web` directory to your server using a tool like `rsync` or `scp`.
3. Install the necessary dependencies on the server, such as the Appwrite server and the Flutter SDK.
4. Run the Appwrite server on the server using the `appwrite start` command.
5. Serve the Flutter app on the server using a web server like Nginx or Apache.

You can also deploy the application on a self-hosted server or a VPS. In this case, you need to install the necessary dependencies on the server, such as the Appwrite server and the Flutter SDK, and copy the application files to the server.

Once you have deployed the application, you can access it by navigating to its URL in a web browser or running the Flutter app on a mobile device.

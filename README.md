## Overview of the Application

The SimpleAdmin application written in Plang is designed to manage user transactions and communicate through the Nostr protocol. The application can start both as a web server and a window application, listens for messages from a specific admin, processes those messages, and handles user transactions such as adding amounts to user balances. The application structure includes several goals defining different functionalities, such as setting up the database, processing messages, adding amounts to user balances, authenticating users, and handling events. Here's a high-level overview:

1. **Setup.goal**: Initializes the database with two tables, `users` and `transactions`, and calls the `GenerateDummyData` goal to populate initial data.

2. **Start.goal**: Starts the webserver, listens for messages from the admin, and processes them through the `ProcessMessage` goal. 

3. **ProcessMessage.goal**: Reads and processes incoming messages using an LLM system. Depending on the message content, it calls other goals such as `AddAmountOnUser` or `Query`.

4. **AddAmountOnUser.goal**: Handles adding a specified amount to a user's balance and logs the transaction.

5. **Query.goal**: Processes database queries and formats the results before sending them back to the admin.

6. **StartWindow.goal**: Starts a window application and calls the `ui/Dashboard`.

7. **Events.goal**: Sets up an event to authenticate users before any API calls.

8. **AuthenticateUser.goal**: Checks if the user is an admin and handles authentication.


# SimpleAdmin

SimpleAdmin is an application written in the Plang programming language, designed to manage user transactions and communicate through the Nostr protocol. The application can run as a web server or a window application and interacts with a specified admin for processing messages and managing user balances.

## Prerequisites

- Install a Nostr client (e.g., Amethyst for Android or Damus for iOS).
- Find your public Nostr address (starts with `npub....`), which will be required the first time you start the app.

## Installation

To install Plang, follow the instructions in the [Install.md](https://github.com/PLangHQ/plang/blob/main/Documentation/Install.md) or visit the [Plang documentation](https://github.com/PLangHQ/plang/tree/main/Documentation).

## Running the Application

1. **Web Server Mode**:
   - Run the command `plang` in the root folder. This will execute `Start.goal`.

2. **Window Application Mode**:
   - Run the command `plangw StartWindow`.

3. **Building the Application**:
   - Run the command `plang build` in the root directory of your project.

## Goals and Their Functions

### Setup.goal
- Initializes the database with the `users` and `transactions` tables.
- Calls the `GenerateDummyData` goal to populate initial data.

### Start.goal
- Starts the web server.
- Listens for messages from the admin and processes them using the `ProcessMessage` goal.

### ProcessMessage.goal
- Reads and processes incoming messages using an LLM system.
- Calls goals such as `AddAmountOnUser` or `Query` based on message content.

### AddAmountOnUser.goal
- Adds a specified amount to a user's balance and logs the transaction.
- Sends a confirmation message back to the admin.

### Query.goal
- Processes database queries.
- Formats and sends query results back to the admin.

### StartWindow.goal
- Starts the window application and calls the `ui/Dashboard`.

### Events.goal
- Sets up an event to authenticate users before any API calls.

### AuthenticateUser.goal
- Checks if the user is an admin and handles authentication.

## Folder Structure

- **llm/**: Contains system commands for the LLM.
- **api/**: Contains API-related goals.
- **ui/**: Contains UI-related goals.
- **events/**: Contains event-related goals.
- **.build/**: Contains built files for execution.
- **.db/**: Contains the SQLite database files.

## Usage

- To interact with the app, you will need to use a Nostr client and send messages to the specified admin address.
- First-time setup will require you to provide your Nostr public address.

## Contact and Support

For assistance, please visit the [Plang Discussion forum](https://github.com/orgs/PLangHQ/discussions) or join the [Discord community](https://discord.gg/A8kYUymsDD).

## License

This project is licensed under the MIT License.


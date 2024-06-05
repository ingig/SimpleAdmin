### Overview of the Application

The application is a simple administrative tool written in the Plang programming language. It interacts with the Nostr protocol, a decentralized communication protocol, to manage user transactions and queries. The application is designed to be run as a web server. UI is either using Nostr client or is provided through a window application. Here’s a breakdown of what the application does:

1. **Setup Phase**:
    - Creates necessary database tables (`users` and `transactions`).
    - Populates the database with some dummy data.

2. **Start Phase**:
    - Starts a web server to listen for messages.
    - Processes incoming messages to execute various goals such as adding an amount to a user's balance or querying data.

3. **Transaction Management**:
    - Handles adding amounts to user accounts.
    - Logs transactions in the `transactions` table.

4. **Query Handling**:
    - Processes queries using predefined LLM (Language Model) configurations.
    - Formats and sends query results back to the admin.

5. **User Authentication**:
    - Authenticates users before allowing them to perform certain actions.

6. **Event Handling**:
    - Executes authentication checks before each API call.
    - Sends debug information before and after each goal during development.


# SimpleAdmin

SimpleAdmin is a lightweight administrative tool written in the Plang programming language. It interacts with the Nostr protocol to handle user transactions and data queries.

## Prerequisites

- **Nostr Client**: 
  - [Amethyst](https://play.google.com/store/apps/details?id=com.vitorpamplona.amethyst&hl=en&pli=1) (Android)
  - [Damus](https://apps.apple.com/us/app/damus/id1628663131) (iOS)

## Installation

1. **Clone the Repository**:
    ```sh
    git clone https://github.com/ingig/SimpleAdmin.git
    cd SimpleAdmin
    ```

2. **Install Plang**:
    Follow the installation guide at [Plang Installation](https://github.com/PLangHQ/plang/blob/main/Documentation/Install.md).

3. **Set Up Database**:
    ```sh
    plang exec Setup.goal
    ```

## Running the Application

### Run as Web Server
```sh
plang
```
This will execute the `Start.goal` file and start the web server.

### Run as Window Application
```sh
plangw StartWindow
```

### Build the Application
```sh
plang build
```

## Folder Structure

- **llm**: Contains system commands for the LLM.
- **api**: Contains API-related goals.
- **ui**: Contains user interface goals.
- **events**: Contains event handling goals.
- **.build**: Contains built files.
- **.db**: Contains the SQLite database files.

## Goals Overview

### Setup.goal

Sets up the initial database schema:
```plang
Setup
- create table users, columns: 
    Identity(string, not null), balance(decimal, not null, default 0), 
    created(datetime, now as default)
- create table transactions, columns: 
    userId(long, not null), amount(decimal, not null), type(string, not null)
- call goal GenerateDummyData
```

### Start.goal

Starts the web server and listens for messages:
```plang
Start
- start webserver
- listen for a message from %Settings.Admin%, 
    call !ProcessMessage content=%content%

ProcessMessage
- read llm/processMessageSystem.txt, into %processMessageSystem%
- [llm] system: %processMessageSystem%
        user: %content% 
        scheme: {goal:string, parameters:object}
- call goal %goal%, %parameters%
```

### AddAmount.goal

Handles adding amounts to user accounts:
```plang
AddAmount
- set default value %type%='free'
- begin transaction
- insert into transactions, %userId%(long), %amount%, type='free'
- update users set balance=balance+%amount% where id=%userId%(long)
- end transaction
- write out 'Amount added'
```

### StartWindow.goal

Starts the window application:
```plang
StartWindows
- start window app, call ui/Dashboard
```

### Events.goal

Defines event handling for API calls:
```plang
Events
- before each goal in api/*, call AuthenticateUser
```

### AuthenticateUser.goal

Authenticates the user:
```plang
AuthenticateUser
- if %Identity% is not same as %Settings.Admin%, then
    - write out error 'Good bye'
```

## License

This project is licensed under the LGPL License - see the [LICENSE](LICENSE) file for details.


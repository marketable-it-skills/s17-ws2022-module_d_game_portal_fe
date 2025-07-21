# Test Project Outline – Module D – Front-End Development

## Competition time

4 hours

## Introduction

A new founded company is looking for full stack developers to create an online browser gaming platform. Game developers can upload their games to the platform and users can play them online in the browser.

There are three parts to the platform:

- Game Developer Portal: A web application for game developers to upload their games to the platform.
- Administrator Portal: A web application for administrators to manage the platform with its users and games.
- Gaming Portal: A web application for players to play games online in the browser.

The company wants to create a minimum viable product (MVP) for the platform. The MVP should already contain the aforementioned parts, but it is acceptable that the Game Developer Portal and the Administrator Portal are not fleshed out yet. The Gaming Portal should be fully functional, so that users can play games online in the browser.

## General Description of Project and Tasks

This project focuses on Phase Two of the gaming platform development, which requires implementing the Developer and Gaming portal frontend. The frontend should be implemented as a single page application using the framework of choice.

The project is split into two phases:
- Phase one (morning) for building the API and static pages using a PHP framework and MySQL database
- Phase two (afternoon) for building the frontend parts using HTML/CSS and a JavaScript framework, consuming the API developed in phase one

You are now in **phase two**. A backend implementation is provided so you can now focus on the frontend. You are **required** to use the provided version and not use your own backend solution from phase one. The provided API should be placed at `/module_c_solution/`.

The frontend provides the following features:

- User Sign Up, Sign In, Sign Out
- Discover games
- Play game
  - When game ends, post scores
  - View highscores per game
- View a user profile
  - Lists authored games
  - Lists highscores per game
- Manage own games
  - Update title and description
  - Upload new version

## Requirements

### Non-Functional Requirements

- You are free to use the frontend technology of your choice.
- The application should run successfully in the versions of Google Chrome and Mozilla Firefox installed on your machine.
- Accessibility: You are expected to follow accessibility best practices. The following rules must be followed:
  - every input has an associated label or placeholder
  - landmarks are used correctly (header, main, footer)
  - the HTML page is W3C valid
  - each page has a discernible title as document title
  - each button has a discernible text
  - each image has an alt text
  - every clickable element can be reached with the Tab key and interacted with by pressing Enter

### Page Structure

The basic structure of each page should contain:

- Header: Visual bar that includes the App Title: "WorldSkills - Games".
- Page Title: A title after the header.
- User section:
  - If the user is not signed in: Shows a "Sign In" and a "Register" button
  - If the user is signed in: Shows a "Sign Out" button

If the user navigates to a page, the document title (the title shown in the browser tab or window title) must reflect the shown page title.

### Page Implementation Requirements

For each page you have to implement, you are provided with a wireframe mockup. These are guides and should not be followed to the pixel. You can come up with your own layout, color scheme and user experience. However, you have to meet or exceed the functionality that the mockups showcase.

#### Page: Discover Games

This is the main page that opens when the user visits the site. It must be reachable under the path `/XX_module_d/`.

The functionality of this page includes:

- See a list of games. For each game, the following is shown:
  - Title
  - Score Count: Number of scores submitted
  - Description: The description provided by the author.
  - Thumbnail: If available, otherwise a default graphic.
  - The list can be sorted by popularity, recently updated, or title. Both ascending and descending options can be selected by the user.
- The link to the game should have link-only link purpose applied for accessibility.

The games are shown in an infinitely scrolling list. With an infinitely scrolling list, there are no pages, but scrolling the page triggers loading of more games if the scroll position gets close to the end of the currently loaded games.

In infinite scroll mode, there are no pages, but scrolling triggers loading of more games.

- An initial number of games are loaded so they overflow the user's screen.
- If the user scrolls towards the end of the loaded games, more games are loaded.
- It only loads as many games that are needed to render enough items so the user does not see the end of the list. It does not load everything to save bandwidth and server resources.
- Reloading the page in the browser resets the user to the top again, but retains the sort settings. Note that browsers try to retain the current scroll position and might not go to the very top position by itself upon page reload.

#### Page: Sign Up

Here the user can enter username and password to sign up.

The user can enter username and password to sign up on a separate page.

This page can be reached by clicking "Sign Up" in the header.

If the user is already signed in and tries to access this page, they are redirected to the home page (Path: `/XX_module_d/`).

They do not need to provide their password twice as can sometimes be seen on other websites.

The form is validated by the API:

- Username: required, min length 4, max length 60
- Password: required, min length 8, max length 2^

If any input is invalid, a message is shown next to the relevant field what exactly is invalid and what constraint it violated.

Example:

> Password: Length must be at least 8.

If the server responds with a message that the request was not successful, this response message is shown. For example it could be that the username already existed.

If the sign up is successful, the user is immediately signed in as well and redirected to the home page (Path: `/XX_module_d/`).

#### Page: Sign In

Here the user can enter their username and password to sign back in.

The user can enter their username and password to sign in.

This page can be reached by clicking "Sign In" in the header.

If the user is already signed in and tries to access this page, they are redirected to the home page (Path: `/XX_module_d/`).

The form is validated the same way as the Sign Up form.

If the sign in is successful, they are redirected to the home page (Path: `/XX_module_d/`).

#### Page: After Sign Out

This page is shown after the user clicked the "Sign Out" button.

#### Page: Game

This page renders the game in an iframe and shows the current highscores (which are automatically updated).

This page renders the game in an iframe and shows the current top ten highscores. The list updates automatically regularly by polling the server in a 5 second interval. The game description is shown as well.

The user's highest score in that game is interleaved into the highscores. If the user is within the top ten, the current user is highlighted and marked as being the current user (for example by showing it bold). If the current user has a score that ranks below the top ten, then the score is appended, but shown without rank.

When the game run ends, the user is asked if they want to publish their score. This is triggered by the game in the iframe messaging the parent window that the game ended and includes the score in the message.

The message the game has to send to the parent via `window.parent.postMessage(...)` looks like this:

```json
{
  "event_type": "game_run_end",
  "score": 100
}
```

The parent needs to listen to those messages and prompt the user if they want to publish the score. If the user selects "Yes", then the score is posted to the leaderboards. The top leaderboard that is shown on the page is updated accordingly and immediately. It should not be waited until the leaderboard updates anyway by the interval.

#### Page: User Profile

This page shows the user profile with:

- Username
- Authored Games
  - The list is sorted by when the last version was uploaded. The most recently updated game is at the top.
  - If the user has not uploaded any games, the "Authored Games" section is omitted.
  - Per authored game, the following is shown:
    - Game Title
    - Description
    - Thumbnail
    - The number of submitted scores
- The author is not shown per game as this is redundant information.
- If the active user is looking at their own profile page, they also see games that have no version yet. They are also offered a button to "Manage Game".
- Highscores
  - The list shows the highest score per game.
  - The list is sorted by game title alphabetically.
  - Each game has a link that can be clicked to go to the game.

#### Page: Manage Game

On this page, developers can update title and description of a game, delete the game, or upload a new version.

On this page, developers can do the following:

- Update title and description.
- Upload a new version. This requires the user to upload a ZIP file.
- Delete the game. The game is only deleted after the user confirms.

If a user tries to reach this page without being the author, they are redirected to the game itself.

## Assessment

All assessment is done on server. No assessment process in workstation.

Assessment criteria include:
- Quality of code implementation
- Accessibility compliance and best practices
- Functional requirements completion
- User interface design and user experience
- Browser compatibility (Chrome and Firefox)

## Mark distribution

The project assessment follows the WorldSkills Occupational Standards for Web Technologies with focus on:
- Work organization and self-management
- Communication and interpersonal skills  
- Front-End Development implementation
- User interface design and accessibility
- Code quality and technical implementation

Total available marks as defined in the marking scheme.
<h1 align="center">Orbities – the universe of your focus</h1>

<p align="center">
  <img src="assets/orbities-banner.png" alt="Orbities Banner" width="600">  
</p>

<p align="center">
  <a href="https://apps.apple.com/br/app/orbities/id6753018099?itscg=30200&itsct=apps_box_badge&mttnsubad=6753018099">
    <img src="https://toolbox.marketingtools.apple.com/api/v2/badges/download-on-the-app-store/black/en-us?releaseDate=1759881600"
         alt="Download on the App Store"
         style="width: 245px; height: 62px; object-fit: contain;" />
  </a>
</p>

<p align="center">
  <a href="https://github.com/educcamara">Eduardo</a> (programming) &nbsp;•&nbsp;
  <a href="https://github.com/thalesaraujods">Thales</a> (programming) &nbsp;•&nbsp;
  <a href="https://github.com/naraarrt">Taynara</a> (design)
</p> 

🇧🇷 [PT-BR Version](https://github.com/educcamara/Orbities/tree/pt-br)

## About

Coming from a university context, we saw how difficult it was for us and our friends to stay consistent in our studies, heavily affected by procrastination. With that in mind, we studied the causes and consequences of procrastination in the context of university students, aiming to avoid a decline in academic performance.

With this, **Orbities** was created.

Orbities is an app built (initially) for iPad, where, through automatic planning of group study sessions, these students can avoid the burden of planning everything from scratch and gain motivation to study through group collaboration. Our app can:

- Register activities, specifying what they are about and defining a deadline to finish them
- Automatically plan study sessions using language models, where, based on the title and description of the activity, the app creates a plan of study sessions, each focused on a subtopic
- Register questions for each activity
- Allow sharing and collaboration among friends so they can perform and answer the sessions together
- Propose dynamics in which part of the questions is selected for people to discuss with each other

Already in development:
- Automatic question generation based on the subject
- Assignment of questions to specific sessions

## Technologies and Things

- App built in **Swift**
- Frontend modeled in **SwiftUI**
- Persistence and sync with **CloudKit** for sharing and near real-time updates with other collaborators
- Use of LLM APIs: **Gemini** (Google) and **Foundation Models** (Apple)
- **Combine** for parts that require a more reactive model
- Simple implementation of **Swift Macros** to automate converting types into schemas used in the Gemini API

- **Clean + MVVM** architecture
- Protocol-oriented programming (for dependency inversion)
- **CI/CD** pipeline through **Xcode Cloud**
- **GitFlow** + **Conventional Commits** in the Git workflow

## My Contributions

### MVVM in SwiftUI

Responsible for applying the **MVVM** pattern in the presentation layer with SwiftUI, separating Views from business logic and ensuring that each screen is guided by a clear ViewModel with well-defined states and effects.

### Clean Architecture

Modeling and implementing (as a team) the file and directory structure of the project in Xcode, following Clean Architecture. The main motivation was to modularize the app more easily, enable separate testing, and simplify dependency injection in the frontend layer.

### Xcode Cloud Pipeline

Implementation of a simple **Xcode Cloud** pipeline to handle build, test and archive, as well as TestFlight submission whenever a tag following the `v*` pattern is created in the repository.

Additionally, configuration and maintenance of scripts used during the pipeline through the `ci_scripts` directory.

### CloudKit in the Context of Clean

Implementation of the **CloudKit** data provider service in the external layer of the Clean Architecture, including:

- Creation of DTOs to convert CloudKit-compatible objects into the entities of the innermost layer of the app (Domain Layer)
- Implementation of a cache service to store scope information and CloudKit records, avoiding state loss during app usage
- Implementation of **Subscriptions** for record changes in the cloud using `CKQuerySubscription` and `CKDatabaseSubscription`, enabling:
  - Handling remote notifications to update the app state in (almost) real time
  - Sending and processing push notifications related to data changes

### Gemini API in the Context of Clean

As with CloudKit, the logic was abstracted through DTOs and protocols (following the principles of Clean Architecture), with a direct communication layer through an entity that encapsulates `URLSession` to communicate with the Gemini API via HTTP.

### Swift Macros

**Motivation:**  
A module of the app required the Gemini API to return a [structured output](https://ai.google.dev/gemini-api/docs/structured-output?example=recipe), where it would be necessary, for every type with that behavior, to manually define a JSON-like structure.

**Solution:**  
I implemented a macro (`@GeminiResponseSchema`) that automatically creates a computed property containing a `Dictionary<String, Any>` (JSON-style) describing the type structure, following the official Gemini API documentation.

### Request Coordinator for Performance

**Motivation:**  
Several screens in the flow hierarchy could trigger the same request, which could make the user, when navigating to a screen, execute the same call again from the previous screen, besides generating conflicts and redundant requests.

**Solution:**  
I created the `FetchCoordinator`, an entity that coordinates ongoing tasks, implementing concurrency policies such as **coalesce** and **force refresh**, and integrating with the cache (`cache-else-fetch`, `stale-while-revalidate`), avoiding unnecessary requests.

### Pub/Sub Architecture for Communication Between Screens

**Motivation:**  
Facilitate updating and passing data between screens in the navigation flow, as well as handling update notifications coming from CloudKit, so the screen could update discreetly without direct coupling between components.

**Solution:**  
I created a wrapper around `NotificationCenter` to send and receive specific notifications, where each notification has its definition through the `EventNotification` protocol, which defines a standard payload for that type of event. This brought more consistency and type safety compared to using `NotificationCenter` directly.

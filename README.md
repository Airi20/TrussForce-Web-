## TrussForce Web App – Dev Story 🍵
(Currently only working locally; not yet deployed to the cloud)  

[🇯🇵 日本語](README.jp.md) | [🇺🇸 English](README.md)  

This is a simple and intuitive Web app that automatically calculates member forces in a truss structure (or will be, eventually).
Once you enter the nodes and members, the app computes the reaction forces and axial forces (tension/compression) for each member.
At first, I was satisfied with a .jar running locally—but my curiosity to touch both front-end and back-end dragged me into this beautiful chaos.

## 🧭 Background
I created a Web app to automatically calculate member forces in truss structures.
It worked perfectly in my local environment, but I wanted to let others try it too—so I needed to make it public on the web.

![画面1](スクリーンショット%202025-06-21%20213143.png)
![画面2](スクリーンショット%202025-06-21%20213159.png)



## 🔥 Features
- Single-page app built with React

- Easily add/remove nodes and members

- Intuitive support and load settings

- Real-time calculation results

- Sorted and labeled member forces for easy understanding

- Connected to a Java back-end API for structural calculations

## 🚀 How to Use
- Input nodes and members

- Set supports and loads

- Press “Submit” to calculate and view results

## 🛠️ Tech Stack
- Front-end: React

- Back-end: Java (Spring Boot + custom solver)

- API: JSON via fetch

## 🌐 TrussForce System Flow (Web version)
A breakdown of how the app flows from front-end to back-end, through calculations, and back to the UI.

## 🔁 Process Flow
```
[React UI]
    ↓
POST via fetch (JSON: nodes + members)
    ↓
Spring @RestController
    ↓
Mapped to InputDto via @RequestBody
    ↓
Service class calls Solver
    ↓
TrussForceSolver (calculates reactions & forces)
    ↓
ResultDto constructed
    ↓
JSON response returned to frontend
    ↓
Result rendered on UI
```


## 🧱 Component Details
## 1. React UI (App.js)
Users input node coordinates, supports, loads, and member connections

Submits via:

```
fetch("/api/solve", {
  method: "POST",
  body: JSON.stringify(data),
})
```
- Response is parsed with setResult() and rendered

## 2. Spring Boot Controller (TrussForceController.java)
```
@PostMapping("/api/solve")
public ResultDto solve(@RequestBody InputDto input) {
    return trussForceService.solve(input);
}
```
- Handles POST requests from the front-end

- Parses JSON into InputDto

- Delegates logic to the Service layer

## 3. Service Layer (TrussForceService.java)
```
public ResultDto solve(InputDto inputDto) {
    return solver.solve(inputDto);
}
```
- (Optional) converts DTOs into internal models

- Calls the solver with input

## 4. Solver (TrussForceSolver.java)
```
public ResultDto solve(InputDto dto) {
    // calculate reactions
    // calculate member forces
    return new ResultDto(...);
}
```
- Core Java logic

- Checks determinacy, solves linear system, and computes axial forces

## 5. Result DTO (ResultDto.java)
```
Map<Integer, Double> reactionsX
Map<Integer, Double> reactionsY
Map<String, Double> memberForces
```
Used to structure and send calculation results back to the front-end

## 🧪 Extra Tips
- Controller = API gatekeeper

- Service = traffic controller

- Solver = brain

- DTO = data box

- React = UI & logic

- fetch = bridge between worlds

## 💡 Recommended For:
- Students studying structural mechanics

- High schoolers curious about force analysis

- Engineers needing a quick truss check

- Anyone tired of hand-calculating axial forces

- Classmates suffering through beam/truss assignments

## 😵‍💫 What I Struggled With
- Only working locally
Others couldn’t access the API running on localhost.

- Cloud deployment
Cloud services like AWS/Azure were intimidating or required a credit card (which I didn’t have 😭).

## 📘 What I Learned
- Uploading to GitHub alone doesn’t make an app “public”

- Cloud deployment is key for sharing your app

If your goal is web access, start thinking cloud-first
(You may not even need a local test if you're cloud-bound from the start)

## ✅ Summary
Even if local-only works for you, deploying to the cloud is a must if you want to share your work.
Future developers: don’t wait—get your stuff online sooner!

## 💬 Questions Welcome
If you’re a student who desperately needs a truss analysis done, just post your node coordinates, member list, and supports here—I might get back to you with results:

👉https://github.com/Airi20/TrussForce-Web-/issues/new


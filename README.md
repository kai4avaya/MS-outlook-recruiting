GT Go Phish – Microsoft Outlook Recruitment Portal
This repository contains the phishing simulation web page created for Assignment 1: Go Phish in PUBP‑6725 at Georgia Tech. The page is designed to mimic a Microsoft / Outlook recruitment portal integrated with LinkedIn and GitHub, and then reveal the “gotcha” as part of the class exercise.

Educational use only. This project is for an academic security course and must not be deployed or used against real users.


----

Overview
The page is a single HTML file styled with Tailwind CSS (via CDN) and a bit of custom CSS. It presents:

	* A Microsoft Outlook–branded recruitment portal
	* A “Quick Apply with LinkedIn” OAuth-style button
	* A manual application form that collects:

		* First name / last name
		* Email address
		* GitHub profile URL
		* Azure certification credential ID (pre-filled)
		* Resume / CV upload
	* A GitHub Pages–style footer to make the URL look legitimate

When the user either:

	* Clicks the LinkedIn Quick Connect button, or
	* Submits the manual form

the UI transitions into a “hacker console” view that:

	1. Reveals that this is a GT Assignment 1 phishing simulation.
	2. Shows live captured data about the victim’s environment.
	3. Displays captured form input (if the form was submitted).
	4. Shows a pre-compiled OSINT profile for the target.
	5. Ends with a security reminder about phishing and OAuth abuse.

----

Features
1. Visual Spoofing & Branding
	* Microsoft logo and Outlook branding in the top navigation.
	* Clean, modern layout using Tailwind utility classes.
	* GitHub partnership / GitHub Pages–style footer to reinforce legitimacy.
	* Custom scrollbar styling for a polished, “production” look.

2. Social Engineering Flow
	* Primary call-to-action: “Quick Apply with LinkedIn” (OAuth-style).
	* Secondary path: manual form entry (“or enter details manually” divider).
	* Pre-filled Azure certification ID to increase trust (“from your invitation link”).
	* Resume upload area with drag-and-drop UI.

3. Tracking & Data Capture
The script collects and displays:

	* Time on page before the user acts.
	* Timestamp of the action.
	* IP-based geolocation via https://ipapi.co/json/:

		* IP address
		* City
		* Country
	* Client environment details:

		* Screen resolution
		* Browser language
		* User agent string

If the user submits the form, the following fields are echoed back:

	* First name
	* Last name
	* Email
	* GitHub profile URL
	* Target certification ID

Note: No data is sent to a backend; everything is processed and displayed client-side for demonstration.


4. URL Manipulation
On load, the script calls:

const uniqueId = "req-7829-fc-" + Math.random().toString(36).substr(2, 6);
window.history.replaceState(null, '', '/Microsoft-Outlook/recruitment/' + uniqueId);

This makes the GitHub Pages URL appear as a deep corporate recruitment link, illustrating how attackers can make phishing URLs look more convincing without changing the actual domain.

5. “Gotcha” Reveal Console
After the user interacts:

	* The main white “application” card and navigation/footer are hidden.
	* The body background switches to black.
	* A terminal-style console appears with:

		* [LIVE CAPTURED DATA]
		* [CAPTURED FORM INPUT]
		* [PRE-COMPILED OSINT PROFILE] for the target (education, experience, certs, skills, LinkedIn activity).
	* A final security reminder reinforces best practices:

		* Verify URLs and sources.
		* Be cautious with OAuth (LinkedIn, etc.).
		* Don’t trust branding alone.

----

File Structure
This project is a single-page app:

	* index.html – Contains:

		* HTML structure
		* Tailwind CDN link
		* Custom CSS in <style> tag
		* All JavaScript logic in a <script> tag at the bottom

There is no backend and no external JS build step.

----

How It Works (JavaScript)
Key functions:

	* showLoading(text, duration, callback)


		* Shows a loading overlay (“Connecting to LinkedIn…”, “Verifying Azure Credential…”) for realism.
		* After a delay, hides the overlay and calls callback().
	* fetchIPData()


		* Fetches IP and geolocation data from ipapi.co.
		* Falls back to placeholder values if blocked.
	* triggerGotcha(sourceType, event)


		* sourceType is either 'linkedin' or 'form'.
		* Prevents default form submission when needed.
		* Calculates time on page.
		* Collects form input if sourceType === 'form'.
		* Waits for IP data while the loading overlay is visible.
		* Hides the original UI, switches to dark theme, and populates the “gotcha” console.
	* handleLinkedInConnect()


		* Click handler for the LinkedIn button; calls triggerGotcha('linkedin').
	* handleFormSubmit(e)


		* onsubmit handler for the form; calls triggerGotcha('form', e).

----

Running the Project
Local
	1. Clone the repository:

git clone https://github.com/<your-org>/<your-repo>.git
cd <your-repo>

	2. Open index.html directly in your browser, or serve it with a simple HTTP server:

python -m http.server 8000
# then visit http://localhost:8000


GitHub Pages
	1. Push this repository to GitHub.
	2. Enable GitHub Pages in the repository settings (e.g., main branch, /root).
	3. Visit the published URL (e.g., https://<user>.github.io/<repo>/).

The script will automatically rewrite the path to something like:

/Microsoft-Outlook/recruitment/req-7829-fc-xxxxxx

to simulate a deep corporate portal.

----

Ethical & Legal Notice
	* This project is part of a controlled academic exercise.
	* Do not:

		* Use this page to phish real users.
		* Collect real credentials or sensitive data.
		* Deploy it in any context that violates laws, policies, or terms of service.

Use it only for classroom demonstration, local testing, or in environments where all participants have given informed consent.

----

License
Specify your license here (e.g., MIT, Apache 2.0, or “All rights reserved for course use only”).

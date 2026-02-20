██     ██  ███    ███  ██████  
██     ██  ████  ████  ██   ██ 
██  █  ██  ██ ████ ██  ██████  
██ ███ ██  ██  ██  ██  ██   ██ 
 ███ ███   ██      ██  ██   ██
 

**YouTube Video Tracking via GTM → GA4 → Google Ads
****Overview**
This setup tracks YouTube iframe video engagement and sends data to:
Google Tag Manager (GTM)
Google Analytics 4 (GA4)

Google Ads
Tracked actions:
start (video starts playing)
pause (video paused)
progress (25%, 50%, 75%, 90%)
complete (100% watch)

The script pushes this event to dataLayer:
event: video_engagement
video_action
video_title
video_id
video_percent
video_duration
video_url
video_provider

1. Website Implementation
Add the provided YouTube tracking script:

Option A: Inside website <head>
Option B: Inside GTM → Custom HTML Tag (All Pages)

The script:
Loads YouTube Iframe API
Adds enablejsapi=1 automatically
Tracks milestones (25, 50, 75, 90)
Pushes clean structured event to dataLayer

2. Google Tag Manager (GTM) Setup
STEP 1 — Create Data Layer Variables
Create the following Data Layer Variables:

Name: DL – Video Action
Data Layer Variable Name: video_action

Name: DL – Video Title
Data Layer Variable Name: video_title

Name: DL – Video ID
Data Layer Variable Name: video_id

Name: DL – Video Percent
Data Layer Variable Name: video_percent

Name: DL – Video Duration
Data Layer Variable Name: video_duration

Name: DL – Video URL
Data Layer Variable Name: video_url

STEP 2 — Create Trigger
Trigger Type: Custom Event
Event Name: video_engagement
This trigger fires on: All Custom Events

3. GA4 Setup
STEP 1 — Create GA4 Event Tag in GTM

Tag Type: Google Analytics – GA4 Event
Configuration Tag: Select your GA4 config tag

Event Name (recommended structure):

If you want dynamic mapping:
video_engagement

OR better structured:

Create 4 separate tags using conditions:

video_start → when video_action equals start
video_pause → when video_action equals pause
video_progress → when video_action equals progress
video_complete → when video_action equals complete

Add Event Parameters:
Parameter Name → Value

video_title → {{DL – Video Title}}
video_id → {{DL – Video ID}}
video_percent → {{DL – Video Percent}}
video_duration → {{DL – Video Duration}}
video_url → {{DL – Video URL}}
video_action → {{DL – Video Action}}

Attach Custom Event Trigger.

STEP 2 — Mark Conversion in GA4 (Optional)

Go to GA4:

Admin → Events
Find event: video_complete
Turn ON → Mark as Conversion

4. Google Ads Conversion Setup

OPTION A — Import From GA4 (Recommended)

Google Ads → Tools → Conversions
Click Import → GA4
Select: video_complete

Best practice:
Import instead of firing Google Ads tag directly.

OPTION B — Direct Google Ads Tag in GTM

Create Tag:
Google Ads Conversion Tracking

Trigger:
Custom Event = video_engagement
Condition:
video_action equals complete


5. Testing Checklist
In GTM Preview:
✓ Start video → event fires
✓ 25% → event fires
✓ 50% → event fires
✓ 75% → event fires
✓ 90% → event fires
✓ Video end → complete fires


In GA4 DebugView:
✓ Events received
✓ Parameters populated correctly



Example Final Event Structure
{
"event": "video_engagement",
"video_action": "progress",
"video_title": "Demo Video",
"video_id": "abc123",
"video_percent": 50,
"video_duration": 240,
"video_url": "https://youtube.com/watch?v=abc123
",
"video_provider": "youtube"
}

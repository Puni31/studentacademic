Scalable in-app Notification System

1) A new course is available for the subset of students
I.	courseavailableApi:

Representation in Json:
{
  "id": string,
  "name": string,
  "section": string,
  "descriptionHeading": string,
  "description": string,
  "room": string,
  "ownerId": string,
  "creationTime": string,
  "updateTime": string,
  "enrollmentCode": string,
  "courseState": enum (CourseState),
  "alternateLink": string,
  "teacherGroupEmail": string,
  "courseGroupEmail": string,
  "teacherFolder": {
    object (DriveFolder)
  },
  "courseMaterialSets": [
    {
      object (CourseMaterialSet)
    }
  ],
  "guardiansEnabled": boolean,
  "calendarId": string
}
{
  "title": string,
  "materials": [
    {
      object (CourseMaterial)
    }
  ]
}
{

  // Union field material can be only one of the following:
  "driveFile": {
    object (DriveFile)
  },
  "youTubeVideo": {
    object (YouTubeVideo)
  },
  "link": {
    object (Link)
  },
  "form": {
    object (Form)
  }
  // End of list of possible types for union field material.
}
 Explanation:

State of The Course:
Above code is the json representation for the required api. An automatic notification is generated by the owner of the id and reached to the subset of students regarding the start of the new course.
Provinced - The course has been created, but not yet activated. It is accessible by the primary teacher and domain administrators, who may modify it or change it to the ACTIVE or DECLINED states. A course may only be changed to PROVISIONED if it is in the DECLINED state.
Declined - The course has been created, but declined. It is accessible by the course owner and domain administrators, though it will not be displayed in the web UI. You cannot modify the course except to change it to the PROVISIONED state. A course may only be changed to DECLINED if it is in the PROVISIONED state.
Suspended
The course has been suspended. You cannot modify the course, and only the user identified by the OwnerId can view the course. A course may be placed in this state if it potentially violates the Terms of Service.
Course Material sets []-
The set of courses that are available on the “about” page of this course. 



2) A class wide notification triggered by the staff.
Api name – Notificationtrigger
Code for Notification:
if (!'Notification' in window) {
  // Notifications aren't supported
  return;
}
function isNewNotificationSupported() {
    if (!window.Notification || !Notification.requestPermission)
        return false;
    if (Notification.permission == 'granted')
        throw new Error('You must only call this \*before\* calling
Notification.requestPermission(), otherwise this feature detect would bug the
user with an actual notification!');
    try {
        new Notification('');
    } catch (e) {
        if (e.name == 'TypeError')
            return false;
    }
    return true;
}
if (window.Notification && Notification.permission == 'granted') {
    // We would only have prompted the user for permission if new
    // Notification was supported (see below), so assume it is supported.
    doStuffThatUsesNewNotification();
} else if (isNewNotificationSupported()) {
    // new Notification is supported, so prompt the user for permission.
    showOptInUIForNotifications();
}


Explanation:
What this basically boils down to, is when you receive a push message, you can create a notification with some data, then in the notificationclick event you can get the notification that was clicked and get its data.
An example use case would be a chat application where a user sends multiple messages and the recipient displays multiple notifications. Ideally the web app would be able to notice you have several notifications which haven't been viewed and collapse them down into a single notification.
Without getNotifications() the best you can do is replace the previous notification with the latest message. With getNotifications(), you can "collapse" the notifications if a notification is already displayed - leading to a much better user experience.
A vibration pattern can either be an array of numbers, or a single number which is treated as an array of one number. The values in the array represent times in milliseconds, with the even indices (0, 2, 4, ...) being how long to vibrate, and the odd indices being how long to pause before the next vibration.










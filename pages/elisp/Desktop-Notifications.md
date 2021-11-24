

### 40.19 Desktop Notifications

Emacs is able to send *notifications* on systems that support the freedesktop.org Desktop Notifications Specification and on MS-Windows. In order to use this functionality on POSIX hosts, Emacs must have been compiled with D-Bus support, and the `notifications` library must be loaded. See [D-Bus](../dbus/index.html#Top) in D-Bus integration in Emacs. The following function is supported when D-Bus support is available:

### Function: **notifications-notify** *\&rest params*

This function sends a notification to the desktop via D-Bus, consisting of the parameters specified by the `params` arguments. These arguments should consist of alternating keyword and value pairs. The supported keywords and values are as follows:

*   `:bus bus`

    The D-Bus bus. This argument is needed only if a bus other than `:session` shall be used.

*   `:title title`

    The notification title.

*   `:body text`

    The notification body text. Depending on the implementation of the notification server, the text could contain HTML markups, like ‘`"<b>bold text</b>"`’, hyperlinks, or images. Special HTML characters must be encoded, as ‘`"Contact &lt;postmaster@localhost&gt;!"`’.

*   `:app-name name`

    The name of the application sending the notification. The default is `notifications-application-name`.

*   `:replaces-id id`

    The notification `id` that this notification replaces. `id` must be the result of a previous `notifications-notify` call.

*   `:app-icon icon-file`

    The file name of the notification icon. If set to `nil`, no icon is displayed. The default is `notifications-application-icon`.

*   `:actions (key title key title ...)`

    A list of actions to be applied. `key` and `title` are both strings. The default action (usually invoked by clicking the notification) should have a key named ‘`"default"`’. The title can be anything, though implementations are free not to display it.

*   `:timeout timeout`

    The timeout time in milliseconds since the display of the notification at which the notification should automatically close. If -1, the notification’s expiration time is dependent on the notification server’s settings, and may vary for the type of notification. If 0, the notification never expires. Default value is -1.

*   `:urgency urgency`

    The urgency level. It can be `low`, `normal`, or `critical`.

*   `:action-items`

    When this keyword is given, the `title` string of the actions is interpreted as icon name.

*   `:category category`

    The type of notification this is, a string. See the [Desktop Notifications Specification](https://developer.gnome.org/notification-spec/#categories) for a list of standard categories.

*   `:desktop-entry filename`

    This specifies the name of the desktop filename representing the calling program, like ‘`"emacs"`’.

*   `:image-data (width height rowstride has-alpha bits channels data)`

    This is a raw data image format that describes the width, height, rowstride, whether there is an alpha channel, bits per sample, channels and image data, respectively.

*   `:image-path path`

    This is represented either as a URI (‘`file://`’ is the only URI schema supported right now) or a name in a freedesktop.org-compliant icon theme from ‘`$XDG_DATA_DIRS/icons`’.

*   `:sound-file filename`

    The path to a sound file to play when the notification pops up.

*   `:sound-name name`

    A themable named sound from the freedesktop.org sound naming specification from ‘`$XDG_DATA_DIRS/sounds`’, to play when the notification pops up. Similar to the icon name, only for sounds. An example would be ‘`"message-new-instant"`’.

*   `:suppress-sound`

    Causes the server to suppress playing any sounds, if it has that ability.

*   `:resident`

    When set the server will not automatically remove the notification when an action has been invoked. The notification will remain resident in the server until it is explicitly removed by the user or by the sender. This hint is likely only useful when the server has the `:persistence` capability.

*   `:transient`

    When set the server will treat the notification as transient and by-pass the server’s persistence capability, if it should exist.

*   *   `:x position`
    *   `:y position`

    Specifies the X, Y location on the screen that the notification should point to. Both arguments must be used together.

*   `:on-action function`

    Function to call when an action is invoked. The notification `id` and the `key` of the action are passed as arguments to the function.

*   `:on-close function`

    Function to call when the notification has been closed by timeout or by the user. The function receive the notification `id` and the closing `reason` as arguments:

    *   `expired` if the notification has expired
    *   `dismissed` if the notification was dismissed by the user
    *   `close-notification` if the notification was closed by a call to `notifications-close-notification`
    *   `undefined` if the notification server hasn’t provided a reason

Which parameters are accepted by the notification server can be checked via `notifications-get-capabilities`.

This function returns a notification id, an integer, which can be used to manipulate the notification item with `notifications-close-notification` or the `:replaces-id` argument of another `notifications-notify` call. For example:

```lisp
(defun my-on-action-function (id key)
  (message "Message %d, key \"%s\" pressed" id key))
     ⇒ my-on-action-function
```

```lisp
```

```lisp
(defun my-on-close-function (id reason)
  (message "Message %d, closed due to \"%s\"" id reason))
     ⇒ my-on-close-function
```

```lisp
```

```lisp
(notifications-notify
 :title "Title"
 :body "This is <b>important</b>."
 :actions '("Confirm" "I agree" "Refuse" "I disagree")
 :on-action 'my-on-action-function
 :on-close 'my-on-close-function)
     ⇒ 22
```

```lisp
```

```lisp
A message window opens on the desktop.  Press ``I agree''.
     ⇒ Message 22, key "Confirm" pressed
        Message 22, closed due to "dismissed"
```

### Function: **notifications-close-notification** *id \&optional bus*

This function closes a notification with identifier `id`. `bus` can be a string denoting a D-Bus connection, the default is `:session`.

### Function: **notifications-get-capabilities** *\&optional bus*

Returns the capabilities of the notification server, a list of symbols. `bus` can be a string denoting a D-Bus connection, the default is `:session`. The following capabilities can be expected:

*   `:actions`

    The server will provide the specified actions to the user.

*   `:body`

    Supports body text.

*   `:body-hyperlinks`

    The server supports hyperlinks in the notifications.

*   `:body-images`

    The server supports images in the notifications.

*   `:body-markup`

    Supports markup in the body text.

*   `:icon-multi`

    The server will render an animation of all the frames in a given image array.

*   `:icon-static`

    Supports display of exactly 1 frame of any given image array. This value is mutually exclusive with `:icon-multi`.

*   `:persistence`

    The server supports persistence of notifications.

*   `:sound`

    The server supports sounds on notifications.

Further vendor-specific caps start with `:x-vendor`, like `:x-gnome-foo-cap`.

### Function: **notifications-get-server-information** *\&optional bus*

Return information on the notification server, a list of strings. `bus` can be a string denoting a D-Bus connection, the default is `:session`. The returned list is `(name vendor version spec-version)`.

*   `name`

    The product name of the server.

*   `vendor`

    The vendor name. For example, ‘`"KDE"`’, ‘`"GNOME"`’.

*   `version`

    The server’s version number.

*   `spec-version`

    The specification version the server is compliant with.

If `spec_version` is `nil`, the server supports a specification prior to ‘`"1.0"`’.

When Emacs runs on MS-Windows as a GUI session, it supports a small subset of the D-Bus notifications functionality via a native primitive:

### Function: **w32-notification-notify** *\&rest params*

This function displays an MS-Windows tray notification as specified by `params`. MS-Windows tray notifications are displayed in a balloon from an icon in the notification area of the taskbar.

Value is the integer unique ID of the notification that can be used to remove the notification using `w32-notification-close`, described below. If the function fails, the return value is `nil`.

The arguments `params` are specified as keyword/value pairs. All the parameters are optional, but if no parameters are specified, the function will do nothing and return `nil`.

The following parameters are supported:

*   `:icon icon`

    Display `icon` in the system tray. If `icon` is a string, it should specify a file name from which to load the icon; the specified file should be a `.ico` Windows icon file. If `icon` is not a string, or if this parameter is not specified, the standard Emacs icon will be used.

*   `:tip tip`

    Use `tip` as the tooltip for the notification. If `tip` is a string, this is the text of a tooltip that will be shown when the mouse pointer hovers over the tray icon added by the notification. If `tip` is not a string, or if this parameter is not specified, the default tooltip text is ‘`Emacs notification`’. The tooltip text can be up to 127 characters long (63 on Windows versions before W2K). Longer strings will be truncated.

*   `:level level`

    Notification severity level, one of `info`, `warning`, or `error`. If given, the value determines the icon displayed to the left of the notification title, but only if the `:title` parameter (see below) is also specified and is a string.

*   `:title title`

    The title of the notification. If `title` is a string, it is displayed in a larger font immediately above the body text. The title text can be up to 63 characters long; longer text will be truncated.

*   `:body body`

    The body of the notification. If `body` is a string, it specifies the text of the notification message. Use embedded newlines to control how the text is broken into lines. The body text can be up to 255 characters long, and will be truncated if it’s longer. Unlike with D-Bus, the body text should be plain text, with no markup.

Note that versions of Windows before W2K support only `:icon` and `:tip`. The other parameters can be passed, but they will be ignored on those old systems.

There can be at most one active notification at any given time. An active notification must be removed by calling `w32-notification-close` before a new one can be shown.

To remove the notification and its icon from the taskbar, use the following function:

### Function: **w32-notification-close** *id*

This function removes the tray notification given by its unique `id`.

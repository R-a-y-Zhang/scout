# Introduction to Android
## Developing for Android
Android applications are primarily developed using Java or Kotlin,
and are executed on the ART or Android Runtime Environment. The
primary IDE is through Android Studio which is a modified version
fo Intellij specifically designed for Android development.

## Lifecycle
Every application has an entrypoint, and for an Android application
it is called the Launcher Activity. This is Launcher Activity is
defined in the AndroidManifest.xml. This is simply a normal Activity
but one which you have specified should be the entrypoint of your
application.

## API Levels
Versions of the Android releases. This is separate from versions
of the Android OS, which are named after sweets in alphabetical
order. The latest is called Oreo. API versions are variations onto
the Android SDK. Unlike Apple, anything that existed in a previous
version of the Android SDK will work on all future releases.

## What is an Activity?
Every single application has multiple pages. For example, an email
app may have a page to view all the emails in an inbox, a view
to write emails, and another page to adjust your user preferences.
Each of these can be separated into its own Activity, since each
of these parts do things that are different from the others, each
of them should be a separate Activity.

## Lifetime of an Activity
Every Activity extends from either the base class Activity or
AppCompatActivity. The latter is primarily used to backport
some functionalities to older API versions, specifically to allow
the action bar features to API levels 11 or lower [citation needed].
There are other types of Activities you can extend from, depending on
your needs, but it will always be some type of existing activity.

There are seven functions that are relevant to the lifecycle of an
Activity: onCreate(), onStart(), onResume(), onPause(), onStop,
onDestroy(), and onRestart(). For the matters of this part of the
course, we will focus on onCreate() and onResume().

### onCreate()
This is the first method called when an activity starts, followed
by onStart() and onResume().

onCreate() is where the layout is inflated and rendered. Here you will
do all the setup the Activity would need. This is where you would
inflate the views, initialize all access to widgets in the UI,
etc. Whenever you create a new Activity and you override onCreate
(which you should nearly always do), you must call through to the
superclass implementation which you must do if you override any
of the seven functions.

#### Important functions used in onCreate()
```setContentView(int)``` - Inflates activity UI. Here is where you reference
which .xml layout file the Activity will inflate as the main view.

```findViewById(int)``` - This is used to get a widget in your
layout, like Buttons, RecyclerViews, EditTexts, etc.

### onResume()
This is called after onCreate() and will be used to restore activity
state if someone returns to the activity. This means preserving
input on text field, remembering the data presented to them in a listview,

#### Important functions used in onResume()
```onRestoreInstanceState(Bundle)``` - Parses the data contained in the
Bundle which stipulates the application state when you resume.

## Basic of Android XML Layout files
The Layout of any Activity or Fragment is defined in an .xml file.
You could do it in code but that is hugely inefficient and should
never be done.

To define where each widget is place on the screen, you must define
a top-level **Layout** component. The most popular layouts are
**LinearLayout**, **RelativeLayout**, **CoordinatorLayout**, and
**FrameLayout**.

A **FrameLayout** blocks out a certain portion of the area and is able
to display a single UI element.

A **LinearLayout** is able to align all containing widgets in a linear
fashion, either horizontal or vertical (this must be specified).

A **RelativeLayout** is able to place widgets relative to each other,
aligning widgets to parents, the boundaries of other components, etc.
This is extremely flexible and can do everything that the LinearLayout
can do.

A **CoordinatorLayout** is very similar to a RelativeLayout but uses
constraints instead.

### Important fields in layout files
For any widget, whether Layout or otherwise, you **MUST** specify
a width and a height. Two special values are available: _wrap_content_,
which will only make the width and the height as big as the things that
goes into it, or _match_parent_, which will expand the width or height
to be equal to that of the parent. _id_ is used to reference that particular
element anywhere in your code whether it is from an xml file or from
your code. When you want to reference it in your code, you need to
assign that element an id. If you want to have another element be to
the right of it in a RelativeLayout, you need to give it an id. An ID
does not need to be unique to the application it simply has to be unique
to that particular layout. So if you have two xml files, they can
both have ids called "layout". _margin_ and _padding_ are also important
for establishing spacing between elements. You can also just specify
the margin or padding of an element in just one of the four cardinal
directions (top, bottom, left, right) through a different field.

### Accessing UI elements from code
Every UI element has a class in the Android Library which references it
and it matches the name of that element (i.e. a Button has a class
called Button). To reference it, the basic syntax is
```<UI-type> <variable-name> = (<UI-type>) findViewById(R.id.<elementid>);```

---
layout: post
title:  "Tell Don’t Ask principle"
date:   2020-11-17 17:00:50 +0530
categories: blog
---


Tell don’t ask principle says that 

> Instead of asking an instance of class for data and acting on that data, we should tell an instance of class what needs to be done. Data and operations that depend on that data belong in the same object.

Let’s look at some example

**Ask version**

```kotlin
Class emailValidator(private val user: User) {

	// Let's just assume for the sake of this example that once the email is validated
	// we will set this flag to true
	var isEmailValid: Boolean = false

	fun getUsersEmail(): String {
		return user.email
	}

	fun doesEmailContainSpecialCharacter(email: String): Boolean {
		…
	}

	fun doesEmailContainAlphabets(email: String): Boolean {
		…
	}

	fun doesEmailHasAtLeast8Characters(email: String): Boolean {
		…
	}
}

Class Main() {
	val emailValidator = EmailValidator(User())
	with (emailValidator) {
		Val email = getUsersEmail()
		if (email.length > 8 && email.contains(“special characters”) && email.contains(“at least 1 number”)) {
			// validated email
			isEmailValid = true
		} else {
			// In correct email
			isEmailValid = false
		}
	}
}
```

You see the problem in the above code?.

1. Main class knows too much about the email validator class’s internal detail
2. All of the validation logic that Main class is doing should actually belong in EmailValidator class.
3. Main class is using information from the Emailvalidator class data and also making decisions on that object



**Tell version**

```kotlin
Class emailValidator(private val user: User) {
	var isEmailValid: Boolean = false

	fun validateEmail() {
		// validate email based on several criteria
		…

		if (validEmail) {
			isEmailValid = true
		} else {
			isEmailValid = false
		}
		…
	}
}

Class Main() {
	val emailValidator = EmailValidator(User())
	with (emailValidator) {
		validateEmail()
	}
}
```

In OOP, it’s always good to tell object what to do rather than querying it and making decisions related to the object. ***It’s not necessary to follow tell, don’t ask principle to the extreme limits. it’s ok to query an object for its state, provided the information isn’t being used to make a decision related to the object***

**Until next time**


📘 CUG Library Integration Guide



Welcome to the CUG Library — a lightweight Kotlin/Android SDK for integrating with the Closed User Group API.
This guide will show you how to add and use the library in your Android project.



⚙️ 1. Add the Maven Repository



In your settings.gradle, add the following inside dependencyResolutionManagement:



dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        // 👇 Add Closed Maven Repo
        maven { url = uri("https://kwesiamartey.github.io/maven-repo/") }
    }
}





📦 2. Add Dependency in build.gradle (App Module)



Open your app/build.gradle and add:
dependencies {
    implementation("com.closed:cuglibrary:1.0.0")
}





🪄 3. Initialize the SDK



Initialize the SDK in your Application class or inside an Activity before making any API calls:



import emmanuel.sackey_kyere.closedusergroup.closedAPI
import emmanuel.sackey_kyere.cuglibrary.ApiServices



lateinit var api: ApiServices



api = ClosedAPI.init(context)





🔐 4. Security Built-In



Each request includes encrypted headers for date-based authentication.



Only authorized apps (verified via SHA1 fingerprint) can access the API.



Unauthorized usage will throw:



SecurityException("Unauthorized application")



💬 5. Example Usage
🔹 Login User



val loginData = SignInData(username = "john", password = "secret")



api.signInUser(loginData).enqueue(object : Callback<SignInResponse> {
    override fun onResponse(call: Call<SignInResponse>, response: Response<SignInResponse>) {
        if (response.isSuccessful) {
            val user = response.body()
            Log.d("closedAPI", "Login successful: ${user?.message}")
        }
    }



    override fun onFailure(call: Call<SignInResponse>, t: Throwable) {
        Log.e("closedAPI", "Login failed: ${t.message}")
    }
})





💰 6. Supported Endpoints
        Function  Description
        signUpUser()  Register new users
        signInUser()  Authenticate existing users
        getBalanceWallet()  Fetch wallet balance
        loadWallet()  Load funds into wallet
        getAllTransaction()  Fetch transaction history
        verifyPhone()  Verify phone number
        getBookingDate()  Get booking availability
        etc





🚫 7. Common Errors
        Error  Meaning  Fix
        SecurityException("Unauthorized application")  App’s SHA1 isn’t whitelisted  Contact admin for authorization
        HTTP 401  Invalid or expired credentials  Ensure login and tokens are correct
        Network Error  Timeout or no internet  Check connectivity or base URL

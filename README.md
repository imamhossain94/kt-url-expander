# Kotlin-URL-Expander

```kotlin
object URLExpander {
    fun expand(url: String): String {
        var connection: HttpURLConnection
        var finalUrl = url
        try {
            do {
                connection = URL(finalUrl).openConnection() as HttpURLConnection
                connection.instanceFollowRedirects = false
                connection.useCaches = false
                connection.requestMethod = "GET"
                connection.connect()
                val responseCode = connection.responseCode
                if (responseCode in 300..399) {
                    val redirectedUrl = connection.getHeaderField("Location") ?: break
                    finalUrl = redirectedUrl
                } else break
            } while (connection.responseCode != HttpURLConnection.HTTP_OK)
            connection.disconnect()
        } catch (e: Exception) {
            e.printStackTrace()
        }
        return finalUrl
    }
}

```
# Key points
- TO keep your tests repeatable and predicatable, you shouldn't make real network calls in your tests.
- You can use MockWebServer to script request reponses and verify that the correct endpoint was called. 
-- How you create and maintain test data will change depending on the needs for your app. 
- You can mock the network layer with Mockito if you don't need the fine-grained control of MockWebServer. 
- By using the Faker library you can easily create reandom interesting tests data. 
- Deciding which tools are right fo the job takes time to learn through experiment trail and error. 

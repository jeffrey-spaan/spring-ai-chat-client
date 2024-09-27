# Spring AI Chat Client
<small>Using the OpenAI API</small>

A <strong>Spring AI Chat Client</strong> using the OpenAI API serves as an application that allows users to interact with AI-driven conversational agents powered by OpenAI's language models.<br />
The integration enables businesses and developers to create smart chatbots or virtual assistants that can handle customer inquiries, provide information, and enhance user engagement through natural conversation.

<b>Author:</b> <a href="https://github.com/jeffrey-spaan" target="_blank">Jeffrey Spaan</a><br>
<b>Created:</b> 2024-09-27<br>
<b>Last updated:</b> 2024-09-27

[![](https://img.shields.io/badge/Spring%20Boot-8A2BE2)]() [![](https://img.shields.io/badge/release-Sep%2019,%202024-blue)]() [![](https://img.shields.io/badge/version-3.3.4-blue)]()

## 1. Why should you use Spring AI?
1. <b>Rapid Development and Scalability</b>
   Spring Framework provides robust features and a wide range of tools, making it easier to develop and scale applications. The modular architecture allows you to add new functionalities or integrate additional services without significant refactoring.<br><br>
2. <b>Enhanced User Experience</b>
   Natural Conversations, leveraging the OpenAI API, allows for sophisticated natural language processing, enabling more intuitive and engaging interactions. This can lead to improved customer satisfaction and retention.<br><br>
3. <b>Seamless Integration with Existing Systems</b>
   The Spring ecosystem supports microservices architecture, making it simple to integrate the chat client with other services (like databases, user authentication, and third-party APIs), facilitating a cohesive application environment.
   <br><br>

## 2. How a Spring AI Chat Client Implementation Works

With the provided example and guidelines, you can effectively implement a <strong>Spring AI Chat Client</strong> in your <strong>Spring Boot</strong> projects.

### 2.1 Create a Spring AI Chat Client Application
For this basic example we will start with a simple REST endpoint which we can call to see the <strong>Spring AI Chat Client</strong> at work.
Let's create an application using the dependencies as previewed:

![01-start-spring-io](https://github.com/jeffrey-spaan/spring-ai-chat-client/blob/main/images/01-start-spring-io.png)

[![](https://img.shields.io/badge/Spring%20Web-8A2BE2)]()
This Spring Framework dependency will provide us with all the necessary functionality to create and manage our REST endpoints.


[![](https://img.shields.io/badge/OpenAI-8A2BE2)]()
The OpenAI Spring dependency is a library that simplifies the integration of OpenAI's API within a Spring application.

### 2.2 Set Application Properties
To use the OpenAI API, you need to provide your API key. You can do this by adding the following configuration to your `application.yml` file.
<i>If you don't have an API key, you can sign up for one at the <a href="https://platform.openai.com/">OpenAI website</a>.</i>

```properties
spring:
  application:
    name: spring-ai-chat-client
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}
      chat:
        options:
          model: gpt-4o-mini
            max-tokens: 100
```

The <b>OPEANAI_API_KEY</b> is an environment variable that you can set in your system or in your IDE. This way, you can keep your API key secure and avoid exposing it in your codebase.

In this example the gpt-4o-mini `model` is used, but you can choose a different model based on your requirements. The gpt-4o-mini model is the affordable and intelligent small model for fast, lightweight tasks.<br>
<a href="https://openai.com/api/pricing/">See all model specifications and pricing</a><br>
The `max-tokens` parameter specifies the maximum number of tokens that the model can generate in a single response.

### 2.3 Create the Chat Client Configuration
To interact with the OpenAI API, you need to create a configuration class that will provide the necessary beans to your application.

```java
@Configuration
class Config {

   @Bean
   ChatClient chatClient(ChatClient.Builder builder) {
      return builder.defaultSystem("You are a friendly chat bot that answers question in the voice of a {voice}")
              .build();
   }
}
```

The `ChatClient` bean will be used to interact with the OpenAI API. The `ChatClient.Builder` class is provided by the OpenAI Spring library and allows you to configure the chat client with the necessary settings.

In this example configuration, we set the default system message that the chat client will use when starting a conversation. You can customize this message to suit your application's needs.

### 2.4 Create the Chat Client REST Controller
Now, let's create a REST controller that will expose an endpoint to interact with the chat client.

```java
@RestController
class ChatClientController {
    private final ChatClient chatClient;

    ChatClientController(ChatClient chatClient) {
        this.chatClient = chatClient;
    }

    @GetMapping("/chat")
    Map<String, String> completion(@RequestParam(value = "message", defaultValue = "Tell me a joke") String message, String voice) {
        return Map.of("completion",
                chatClient.prompt()
                        .system(sp -> sp.param("voice", voice))
                        .user(message)
                        .call()
                        .content());
    }
}
```

In this controller, we inject the `ChatClient` bean and create a `GET` endpoint `/chat` that accepts a query parameter `message`. The `message` parameter is the user's input, and the `voice` parameter is used to customize the chatbot's response.

The `completion` method calls the `chatClient.prompt()` method to start a conversation. We then set the system parameters using the `system` method and pass the user's message using the `user` method. Finally, we call the `call` method to get the chatbot's response.

### 3 Running the Spring AI Chat Client Application
You can now run your Spring Boot application and access the `/chat` endpoint to interact with the chat client.

- To test the <b>REST endpoint</b>, a tool like <b>Postman</b> can be used to send <b>HTTP requests</b>.
- A Postman collection is added within the repository `src/main/resources/postman/collection-to-import.json`

[![](https://img.shields.io/badge/GET%20-chat%20message-green)]()<br/>
<small>Endpoint:</small> `http://localhost:8080/chat`<br/>
<small>Params:</small><br/>
- `message` (optional): The user's message to the chatbot.<br/>
- `voice` (optional): The voice in which the chatbot responds.<br/>

![02-postman-get-chat](https://github.com/jeffrey-spaan/spring-ai-chat-client/blob/main/images/02-postman-get-chat.png)

## Let's Stay Connected

If you have any questions in regard to this repository and/or documentation, please do reach out.

Don't forget to:
- <b>Star</b> the [repository](https://github.com/jeffrey-spaan/spring-ai-chat-client)
- [Follow me](https://github.com/jeffrey-spaan) for more interesting repositories!
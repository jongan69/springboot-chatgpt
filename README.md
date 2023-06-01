# ChatGPT Spring Boot API

This project is a RESTful API designed with Spring Boot that utilizes OpenAI's GPT-3 model, specifically the ChatGPT variant, to generate human-like text responses.

## Prerequisites

- Java 11 or higher
- Maven
- An OpenAI API key (you can obtain this from [OpenAI's website](https://beta.openai.com/signup/)

# Summary

OpenAIConfig.Java: This is a configuration class for the Spring Framework. It sets up a RestTemplate bean and adds an interceptor to it. This bean is used to make HTTP requests to the OpenAI service, with the OpenAI API key injected into the Authorization header of each request.

CustomBotController.Java: This is a REST controller that handles incoming HTTP GET requests to the /bot/chat URL path. It takes a string parameter prompt from the request URL and creates a ChatGPTRequest object with the model and prompt values. The request is sent to the OpenAI API and the response is mapped to a ChatGptResponse object. The content of the first choice from the response is returned as a string.

ChatGPTRequest.Java: This Data Transfer Object (DTO) class represents a request to the ChatGPT API. It contains the model information and a list of Message objects, where each Message represents a conversation message with a sender and content.

ChatGPTResponse.Java: This DTO class represents the response from the ChatGPT API. It contains a list of Choice objects, each representing a response from the model. Each Choice has an index and a Message object representing the content of the choice.

Message.Java: Message.Java is a DTO that represents a conversation message. It contains a role which indicates the sender's role in the conversation, and content which is the content of the message.

# Config: OpenAIConfig.Java

- The code is a configuration class for the Spring Framework.
- It sets up a RestTemplate bean and adds an interceptor to it.
- The class is annotated with @Configuration.
- It imports various annotations and classes from the Spring Framework.
- The @Value annotation is used to inject values from external sources.
- The openaiApiKey variable holds the value of the property "openai.api.key".
- The @Bean annotation marks a method as a bean definition method.
- The template() method creates and configures a RestTemplate object.
- An interceptor is added to the RestTemplate object.
- The interceptor adds an "Authorization" header to the request with the OpenAI API key.
- The request is executed and the response is returned.
- The configured RestTemplate object is returned as a bean.


# Controller: CustomBotController.Java

- The code is a controller class for the Spring Framework.
- It handles incoming HTTP GET requests to the /bot/chat endpoint.
- The class is annotated with @RestController.
- It imports various annotations and classes from the Spring Framework.
- The ChatGPTRequest and ChatGptResponse classes are imported from the com.chatgpt.dto package.
- The @Autowired annotation is used for automatic dependency injection.
- The @Value annotation is used to inject values from external sources.
- The RestTemplate class is imported from the org.springframework.web.client package.
- The @RequestMapping annotation defines the base URL mapping for the controller.
- The @GetMapping annotation maps HTTP GET requests to a specific method in the controller.
- The @RequestParam annotation binds a request parameter to a method parameter.
- The RestTemplate field is marked for automatic dependency injection.
- The chat() method handles HTTP GET requests and takes a prompt parameter from the query parameter.
- A ChatGPTRequest object is created with the model and prompt values.
- A POST request is sent to the apiURL using the RestTemplate object, with the request object as the body.
- The response is mapped to a ChatGptResponse object.
- The content of the first choice in the response is extracted and returned as a string.

# DTO: ChatGPTRequest.Java, ChatGPTResponse.Java, Message.Java

`ChatGPTResponse.Java`
- The code defines a data transfer object (DTO) class named ChatGPTRequest.
- The class is located in the com.chatgpt.dto package.
- The @Data annotation from the Lombok library is used to generate common methods for the class.
- It has a private instance variable named model of type String, representing the model for the ChatGPT API.
- It has a private instance variable named messages of type List<Message>, representing a list of Message objects.
- The class has a constructor that takes the model and prompt as parameters.
- The constructor assigns the model parameter to the model instance variable.
- The constructor initializes the messages list as an empty ArrayList.
- The constructor adds a new Message object to the messages list, with "user" as the sender and prompt as the content.
- The ChatGPTRequest class is used as a data structure to represent requests to the ChatGPT API, containing the model and conversation messages.

`ChatGPTResponse.Java`
- The code defines two DTO classes: ChatGptResponse and Choice.
- The classes are located in the com.chatgpt.dto package.
- The Lombok library is used to automatically generate boilerplate code.
- The @Data annotation generates getter, setter, equals(), hashCode(), and toString() methods.
- The @AllArgsConstructor annotation generates a constructor with parameters for all fields.
- The @NoArgsConstructor annotation generates a no-argument constructor.
- The ChatGptResponse class has a private instance variable named choices, representing a list of Choice objects.
- The Choice class, defined as an inner class within ChatGptResponse, has private instance variables for index and message.
- The Choice class also utilizes Lombok annotations to generate necessary methods and constructors.
- ChatGptResponse represents the response from the ChatGPT API, containing a list of Choice objects.
- Choice represents an option or response from the model, consisting of an index and a Message object.
-  Lombok annotations reduce the amount of manual code that needs to be written.
  
`Message.Java`
- The code defines a DTO class named `Message`.
- The class is located in the `com.chatgpt.dto` package.
- The Lombok library is used to automatically generate boilerplate code.
- The `@Data` annotation generates getter, setter, equals(), hashCode(), and toString() methods.
- The `@AllArgsConstructor` annotation generates a constructor with parameters for all fields.
- The `@NoArgsConstructor` annotation generates a no-argument constructor.
- The `Message` class has private instance variables for `role` and `content`.
- The `role` variable represents the sender or role of the message.
- The `content` variable represents the content or text of the message.
- Lombok annotations reduce the amount of manual code that needs to be written by automatically generating the necessary methods and constructors.

# ChatGPT Spring Boot API

This project is a RESTful API designed with Spring Boot that utilizes OpenAI's GPT-3 model, specifically the ChatGPT variant, to generate human-like text responses.

## Prerequisites

- Java 11 or higher
- Maven
- An OpenAI API key (you can obtain this from [OpenAI's website](https://beta.openai.com/signup/))

# Config: OpenAIConfig.Java

This code is a configuration class for Spring Framework that sets up a RestTemplate bean and adds an interceptor to it. Let's go through it line by line:

package com.chatgpt.config;

This line specifies the package name where this class belongs.
import org.springframework.beans.factory.annotation.Value;

This line imports the Value annotation from the org.springframework.beans.factory.annotation package. The Value annotation is used to inject values from external sources into Spring beans.
import org.springframework.context.annotation.Bean;

This line imports the Bean annotation from the org.springframework.context.annotation package. The Bean annotation is used to declare a bean in Spring.
import org.springframework.context.annotation.Configuration;

This line imports the Configuration annotation from the org.springframework.context.annotation package. The Configuration annotation indicates that this class is a configuration class for Spring.
import org.springframework.web.client.RestTemplate;

This line imports the RestTemplate class from the org.springframework.web.client package. RestTemplate is a class provided by Spring that simplifies making HTTP requests.
@Configuration

This line marks the class as a configuration class for Spring. It indicates that this class will define bean configurations.
public class OpenAIConfig {

This line defines a public class named OpenAIConfig. It is the configuration class that we are going to explain.
@Value("${openai.api.key}")

This line uses the @Value annotation to inject the value of the property openai.api.key into the openaiApiKey field. The value is retrieved from an external configuration source, such as a properties file or environment variables.
String openaiApiKey;

This line declares an instance variable openaiApiKey of type String. This variable will hold the API key for the OpenAI service.
@Bean

This line marks the following method as a bean definition method. The method annotated with @Bean will create and configure an object to be managed by the Spring container.
public RestTemplate template() {

This line declares a method named template that returns a RestTemplate object. This method will be responsible for creating and configuring the RestTemplate bean.
RestTemplate restTemplate = new RestTemplate();

This line creates a new instance of the RestTemplate class.
restTemplate.getInterceptors().add((request, body, execution) -> {

This line adds an interceptor to the RestTemplate object. Interceptors can modify outgoing requests or incoming responses before they are processed.
request.getHeaders().add("Authorization", "Bearer " + openaiApiKey);

This line adds an Authorization header to the request with the value of "Bearer" followed by the openaiApiKey variable. This header is typically used for authentication purposes.
return execution.execute(request, body);

This line executes the request and returns the response.
return restTemplate;

This line returns the configured RestTemplate object as a bean.
This configuration class sets up a RestTemplate bean and adds an interceptor to it that sets the Authorization header with the OpenAI API key. The RestTemplate bean can then be used in other parts of the application to make HTTP requests to the OpenAI service.



# Controller: CustomBotController.Java

package com.chatgpt.controller;

This line specifies the package name where this class belongs.
import com.chatgpt.dto.ChatGPTRequest;

This line imports the ChatGPTRequest class from the com.chatgpt.dto package. It is a custom class that represents a request object for the ChatGPT API.
import com.chatgpt.dto.ChatGptResponse;

This line imports the ChatGptResponse class from the com.chatgpt.dto package. It is a custom class that represents a response object from the ChatGPT API.
import org.springframework.beans.factory.annotation.Autowired;

This line imports the Autowired annotation from the org.springframework.beans.factory.annotation package. The Autowired annotation is used for automatic dependency injection.
import org.springframework.beans.factory.annotation.Value;

This line imports the Value annotation from the org.springframework.beans.factory.annotation package. The Value annotation is used to inject values from external sources into Spring beans.
import org.springframework.web.bind.annotation.GetMapping;

This line imports the GetMapping annotation from the org.springframework.web.bind.annotation package. The GetMapping annotation is used to map HTTP GET requests to a specific method in the controller.
import org.springframework.web.bind.annotation.RequestMapping;

This line imports the RequestMapping annotation from the org.springframework.web.bind.annotation package. The RequestMapping annotation is used to define a base URL for the controller.
import org.springframework.web.bind.annotation.RequestParam;

This line imports the RequestParam annotation from the org.springframework.web.bind.annotation package. The RequestParam annotation is used to bind a request parameter to a method parameter.
import org.springframework.web.bind.annotation.RestController;

This line imports the RestController annotation from the org.springframework.web.bind.annotation package. The RestController annotation is a specialized version of the Controller annotation that is used to define a RESTful controller.
import org.springframework.web.client.RestTemplate;

This line imports the RestTemplate class from the org.springframework.web.client package. RestTemplate is a class provided by Spring that simplifies making HTTP requests.
@RestController

This line marks the class as a REST controller. It indicates that this class will handle incoming HTTP requests and produce HTTP responses.
@RequestMapping("/bot")

This line specifies the base URL mapping for all the endpoints defined in this controller. In this case, all endpoints will be prefixed with /bot.
@Value("${openai.model}")

This line uses the @Value annotation to inject the value of the property openai.model into the model field. The value is retrieved from an external configuration source, such as a properties file or environment variables.
@Value(("${openai.api.url}"))

This line uses the @Value annotation to inject the value of the property openai.api.url into the apiURL field. The value is retrieved from an external configuration source, such as a properties file or environment variables.
@Autowired

This line marks the RestTemplate field for automatic dependency injection. Spring will automatically instantiate and inject a RestTemplate object into this field.
@GetMapping("/chat")

This line specifies that the following method will handle HTTP GET requests to the /bot/chat URL path.
public String chat(@RequestParam("prompt") String prompt)

This line defines a method named chat that takes a String parameter called prompt, which is annotated with @RequestParam("prompt"). This means that the prompt value will be extracted from the query parameter in the request URL.
ChatGPTRequest request = new ChatGPTRequest(model, prompt);

This line creates a new instance of the ChatGPTRequest class, passing the model and prompt values as arguments. This object represents the request to be sent to the ChatGPT API.
ChatGptResponse chatGptResponse = template.postForObject(apiURL, request, ChatGptResponse.class);

This line sends a POST request to the apiURL using the RestTemplate object (template). The request object is sent as the request body, and the response is mapped to an instance of the ChatGptResponse class. The postForObject method returns the response body as an object of the specified class.
return chatGptResponse.getChoices().get(0).getMessage().getContent();

This line retrieves the content of the first choice from the ChatGptResponse object and returns it as a String. It assumes that the response contains at least one choice.
This controller class handles HTTP GET requests to the /bot/chat endpoint, where the prompt parameter is passed as a query parameter. It uses the injected RestTemplate object to send a POST request to the apiURL with the ChatGPTRequest object as the request body. The response is then processed to extract the content of the first choice and return it as a string.

# DTO: ChatGPTRequest.Java, ChatGPTResponse.Java, Message.Java

ChatGPTRequest.Java

This code defines a data transfer object (DTO) class named ChatGPTRequest in the package com.chatgpt.dto. Let's go through the code line by line:

package com.chatgpt.dto;

This line specifies the package name where this class belongs.
import lombok.Data;

This line imports the Data annotation from the Lombok library. The Data annotation automatically generates boilerplate code, such as getters, setters, equals(), hashCode(), and toString(), for the fields in the class.
@Data

This line applies the Data annotation to the class. It instructs Lombok to generate the necessary getter, setter, and other common methods for the fields in the class.
private String model;

This line declares a private instance variable named model of type String. It represents the model for the ChatGPT API.
private List<Message> messages;

This line declares a private instance variable named messages of type List<Message>. It represents a list of Message objects.
public ChatGPTRequest(String model, String prompt) {

This line defines a public constructor for the ChatGPTRequest class, which takes two parameters: model of type String and prompt of type String. This constructor is used to create an instance of ChatGPTRequest.
this.model = model;

This line assigns the value of the model parameter to the model instance variable.
this.messages = new ArrayList<>();

This line initializes the messages list by creating a new ArrayList object. It ensures that the messages field is not null and can be used to add Message objects.
this.messages.add(new Message("user", prompt));

This line adds a new Message object to the messages list. The Message object is created with the parameters "user" as the sender and prompt as the content.
The ChatGPTRequest class serves as a data transfer object that represents a request to the ChatGPT API. It contains the model information and a list of Message objects, where each Message represents a conversation message with a sender and content.





ChatGPTResponse.Java
  
This code defines two DTO classes, ChatGptResponse and Choice, within the com.chatgpt.dto package. Let's go through the code line by line:

package com.chatgpt.dto;

This line specifies the package name where these classes belong.
import lombok.AllArgsConstructor;

This line imports the AllArgsConstructor annotation from the Lombok library. The AllArgsConstructor annotation generates a constructor with parameters for all fields in the class.
import lombok.Data;

This line imports the Data annotation from the Lombok library. The Data annotation automatically generates boilerplate code, such as getters, setters, equals(), hashCode(), and toString(), for the fields in the class.
import lombok.NoArgsConstructor;

This line imports the NoArgsConstructor annotation from the Lombok library. The NoArgsConstructor annotation generates a no-argument constructor for the class.
@Data

This line applies the Data annotation to the class. It instructs Lombok to generate the necessary getter, setter, and other common methods for the fields in the class.
@AllArgsConstructor

This line applies the AllArgsConstructor annotation to the class. It instructs Lombok to generate a constructor with parameters for all fields in the class.
@NoArgsConstructor

This line applies the NoArgsConstructor annotation to the class. It instructs Lombok to generate a no-argument constructor for the class.
private List<Choice> choices;

This line declares a private instance variable named choices of type List<Choice>. It represents a list of Choice objects.
@Data

This line applies the Data annotation to the inner Choice class. It instructs Lombok to generate the necessary getter, setter, and other common methods for the fields in the class.
@AllArgsConstructor

This line applies the AllArgsConstructor annotation to the inner Choice class. It instructs Lombok to generate a constructor with parameters for all fields in the class.
@NoArgsConstructor

This line applies the NoArgsConstructor annotation to the inner Choice class. It instructs Lombok to generate a no-argument constructor for the class.
private int index;

This line declares a private instance variable named index of type int. It represents the index of the choice.
private Message message;

This line declares a private instance variable named message of type Message. It represents the message associated with the choice.
The ChatGptResponse class represents the response from the ChatGPT API. It contains a list of Choice objects, where each Choice represents an option or response from the model. The Choice class, defined as an inner class within ChatGptResponse, contains an index and a Message object representing the content of the choice.

Both classes utilize Lombok annotations (Data, AllArgsConstructor, and NoArgsConstructor) to generate the necessary boilerplate code automatically, reducing the amount of code that needs to be written.
  
  Message.Java
  
  This code defines a DTO class named `Message` within the `com.chatgpt.dto` package. Let's go through the code line by line:

1. `package com.chatgpt.dto;`
   - This line specifies the package name where this class belongs.

3. `import lombok.AllArgsConstructor;`
   - This line imports the `AllArgsConstructor` annotation from the Lombok library. The `AllArgsConstructor` annotation generates a constructor with parameters for all fields in the class.

4. `import lombok.Data;`
   - This line imports the `Data` annotation from the Lombok library. The `Data` annotation automatically generates boilerplate code, such as getters, setters, `equals()`, `hashCode()`, and `toString()`, for the fields in the class.

5. `import lombok.NoArgsConstructor;`
   - This line imports the `NoArgsConstructor` annotation from the Lombok library. The `NoArgsConstructor` annotation generates a no-argument constructor for the class.

8. `@Data`
   - This line applies the `Data` annotation to the class. It instructs Lombok to generate the necessary getter, setter, and other common methods for the fields in the class.

9. `@AllArgsConstructor`
   - This line applies the `AllArgsConstructor` annotation to the class. It instructs Lombok to generate a constructor with parameters for all fields in the class.

10. `@NoArgsConstructor`
    - This line applies the `NoArgsConstructor` annotation to the class. It instructs Lombok to generate a no-argument constructor for the class.

12. `private String role;`
    - This line declares a private instance variable named `role` of type `String`. It represents the role or sender of the message.

13. `private String content;`
    - This line declares a private instance variable named `content` of type `String`. It represents the content or text of the message.

The `Message` class is a simple DTO class that represents a message in a conversation. It contains `role` and `content` fields to store the sender and content of the message, respectively. Lombok annotations (`Data`, `AllArgsConstructor`, and `NoArgsConstructor`) are used to generate the necessary boilerplate code automatically, reducing the amount of code that needs to be written.

# Message

- 메시지, 국제화 기능이라고 한다. spring boot와 tymeleaf를 사용하면 손쉽게 사용할 수 있다.
- 메시지란 html에 하드코딩```<label>하드코딩</label>``` 처럼 태그에 그냥 짱박아놓은 것을 의미한다. 메시지 기능을 쓰면 하드코딩이 아닌 동적코딩을 할 수 있다!
- 국제화란 Http Header에 Locale에 따라 클라이언트에게(html) 메시지를 다르게 보여주는 것이다. 영어권에서 요청이 들어오면 영어를, 한국어권에서 요청이 들어오면 한국어를 보여준다.

<br>

## 메시지 및 국제화 설정 방법
1. ```src/main/resource/application.properties``` 에 메시지 루트를 정할 수 있다. default가 "messages"이다.

![image](https://user-images.githubusercontent.com/74396651/181905794-f5a55649-7b29-455e-a34e-2387c0e03776.png)

<br>

2. ```src/main/resource``` 안에 ```messages.properties``` 를 만들어 원하는 메시지를 설정할 수 있다.
   - 만약 한국에서 요청이 들어올 때 보여주고 싶은 메시지라면 messages_kr.properties, 영어권이라면 messages_en.properties를 만들어 설정해주면 된다!

![image](https://user-images.githubusercontent.com/74396651/181905830-e2528c02-04f4-437a-ae15-7b4171db39d5.png) 
![image](https://user-images.githubusercontent.com/74396651/181905849-9ec3fb26-f63b-4772-b11e-438da7318467.png)


<br>

## 사용법 : addForm.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <link th:href="@{/css/bootstrap.min.css}"
          href="../css/bootstrap.min.css" rel="stylesheet">
    <style>
        .container {
            max-width: 560px;
        }
    </style>
</head>
<body>

<div class="container">

    <div class="py-5 text-center">
        <h2 th:text="#{page.addItem}" >상품 등록 폼</h2>
    </div>

    <form action="item.html" th:action th:object="${item}" method="post">
        <div>
            <label for="itemName" th:text="#{label.item.itemName}">상품명 표시</label>
            <input type="text" id="itemName" th:field="*{itemName}" class="form-control" placeholder="이름을 입력하세요">
        </div>
        <div>
            <label for="price" th:text="#{label.item.price}">가격 표시</label>
            <input type="text" id="price" th:field="*{price}" class="form-control" placeholder="가격을 입력하세요">
        </div>
        <div>
            <label for="quantity" th:text="#{label.item.quantity}">수량 표시</label>
            <input type="text" id="quantity" th:field="*{quantity}" class="form-control" placeholder="수량을 입력하세요">
        </div>

        <hr class="my-4">

        <div class="row">
            <div class="col">
                <button class="w-100 btn btn-primary btn-lg" type="submit" th:text="#{button.save}">상품 등록</button>
            </div>
            <div class="col">
                <button class="w-100 btn btn-secondary btn-lg"
                        onclick="location.href='items.html'"
                        th:onclick="|location.href='@{/message/items}'|"
                        type="button" th:text="#{button.cancel}">취소</button>
            </div>
        </div>

    </form>

</div> <!-- /container -->
</body>
</html>

```
<br>

- 사용법은 간단하다. 타임르프에서 <code>th:text="#{...}"</code>을 사용하면 된다!
- messages.properties 에서 ```label.item.item=상품명```이라고 설정해 두었기 때문에 렌더링될 때 "상품 표시" -> "상품명"으로 바뀔 것이다!
- 마찬가지로 만약 Locale이 미국이라면 "상품 표시" -> "Item Name" 이라고 바뀔 것이다!

<br>
<hr>
<br>

## 참고

### LocaleResolver
- Locale 방식을 변경하여 LocalResolverr의 구현제츨 변경해 쿠키나 세션 기반의 Locale을 선택할 수 있다! 나중에 찾아보장



<br>
<hr>
<br>

# Validation(유효성 검증)
- 말 그대로 검증하는 것이다! 뭐 아이디를 안 적었든! 이메일 형식이 안맞든! 뭐든!

<br>

## 메시지 설정

![image](https://user-images.githubusercontent.com/74396651/182047919-3ce13b68-0092-4ac5-a3aa-33ef27b02e41.png)

![image](https://user-images.githubusercontent.com/74396651/182047938-02be12c4-2532-4abb-a790-6debcc6fc4c6.png)

- 마찬가지로 application.properties와 사용할 .properties 파일을 설정해야 한다.

<br>

## tymeleaf에서 사용하는 법
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <link th:href="@{/css/bootstrap.min.css}"
          href="../css/bootstrap.min.css" rel="stylesheet">
    <style>
        .container {
            max-width: 560px;
        }
        .field-error {
            border-color: #dc3545;
            color: #dc3545;
        }
    </style>
</head>
<body>

<div class="container">

    <div class="py-5 text-center">
        <h2 th:text="#{page.addItem}">상품 등록</h2>
    </div>

    <form action="item.html" th:action th:object="${item}" method="post">

        <div th:if="${#fields.hasGlobalErrors()}">
            <p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">글로벌 오류 메시지</p>
        </div>

        <div>
            <label for="itemName" th:text="#{label.item.itemName}">상품명</label>
            <input type="text" id="itemName" th:field="*{itemName}"
                   th:errorclass="field-error" class="form-control" placeholder="이름을 입력하세요">
            <div class="field-error" th:errors="*{itemName}">
                상품명 오류
            </div>
        </div>
        <div>
            <label for="price" th:text="#{label.item.price}">가격</label>
            <input type="text" id="price" th:field="*{price}"
                   th:errorclass="field-error" class="form-control" placeholder="가격을 입력하세요">
            <div class="field-error" th:errors="*{price}">
                가격 오류
            </div>
        </div>

        <div>
            <label for="quantity" th:text="#{label.item.quantity}">수량</label>
            <input type="text" id="quantity" th:field="*{quantity}"
                   th:errorclass="field-error" class="form-control" placeholder="수량을 입력하세요">
            <div class="field-error" th:errors="*{quantity}">
                수량 오류
            </div>

        </div>

        <hr class="my-4">

        <div class="row">
            <div class="col">
                <button class="w-100 btn btn-primary btn-lg" type="submit" th:text="#{button.save}">상품 등록</button>
            </div>
            <div class="col">
                <button class="w-100 btn btn-secondary btn-lg"
                        onclick="location.href='items.html'"
                        th:onclick="|location.href='@{/validation/v3/items}'|"
                        type="button" th:text="#{button.cancel}">취소</button>
            </div>
        </div>

    </form>

</div> <!-- /container -->
</body>
</html>
```
- th:if="${#fields.hasGlobalErrors()}" : 에러가 있다.
- th:errors="*{quantity}" : th:if의 편의 버전이다.
- th:errorclass="field-error" : 에러가 있다면 클래스를 추가한다.
- th:field는 정상 상황일 때는 모델 객체의 값을 사용하지만, 오류가 발생하면 FieldError에서 보관하고 있는 값을 사용한다.

<br>

## 개념
- ```BindingResult```는 스프링이 제공하는 검증 오류를 보관하는 객체이다.  BindingResult는 @ModelAttrbute 뒤에 위치해야 한다. BindingResult가 있으면 @ModelAttribute에 데이터 바인딩 시 오류가 발생해도 컨트롤러가 호출된다. 
- ```BindingResult```는 인터페이스이고 ```Errors``` 인터페이스를 상속받고 있다. 실제 넘어오는 구현체는 ```BeanPropertyBindingResult```라는 것인데, 둘 다 구현하고 있으므로 Error를 사용해도 도지만 관례상 BindingResult를 많이 사용한다.
- ```BindingResult에 검증 오류를 적용하는 3가지 방법이다 있다.
   - @ModelAttribute의 객체에 타입 오류 등으로 바인딩이 실패하는 경우 스프링이 FieldError를 생성해서 BindingResult에 넣어준다.
   - 개발자가 직접 넣어준다.
   - @Validator를 사용한다.

<br>

## Controller 코드 설명
- addItemV1 
![image](https://user-images.githubusercontent.com/74396651/182048498-aa5b4741-ab48-41ed-87ae-a82be88897b2.png)
![image](https://user-images.githubusercontent.com/74396651/182048508-38ed4407-2a0f-4a1a-9355-462c817b0a68.png)

<br>

- addItemV2 
![image](https://user-images.githubusercontent.com/74396651/182048519-c4e43e5b-3fea-4f85-8779-1b3d35b91d03.png)

<br>

- addItemV3 
   - V2 + message 이용!
   
- addItemV4 
   - 컨트롤러에서 BindingResult는 검증해야 할 객체(target) 바로 뒤에 위치한다. 따라서 BindingResult는 이미 본인이 검증해야 할 객체를 알고있다.
   - BindingResult가 제공하는 resultValue(), reject()를 사용하면 FieldError, ObjctError를 직접 생성하지 않고 깔끔하게 검증 오류를 다룰 수 있다.

![image](https://user-images.githubusercontent.com/74396651/182048616-e26dd1ed-5480-4575-9268-b9398bf3e2be.png)

<br>

- ### Validator를 분리해 공통 작업을 수행한다.
```
package hello.itemservice.web.validation;

import hello.itemservice.domain.item.Item;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

@Component
public class ItemValidator implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return Item.class.isAssignableFrom(clazz); 
        // item == clazz
        // item == subitem 인지 확인하는 메서드
    }

    @Override
    public void validate(Object target, Errors errors) {
        Item item = (Item) target;

//        if (!StringUtils.hasText(item.getItemName())) {
//            errors.rejectValue("itemName", "required");
//        }
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "itemName", "required");
        
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            errors.rejectValue("price", "range", new Object[]{1000, 10000000}, null);
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            errors.rejectValue("quantity", "max", new Object[]{9999}, null);
        }

        //특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                errors.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }
    }
}

```
<br>

- addItemV5 
   - ItemValidator(Validator 분리)를 스프링 빈으로 주입 받아서 호출했다.
   
- addItemV6 
    - WebDataBinder에 검증기를 추가하여 해당 컨트롤러에서 검증기를 자동으로 적용할 수 있다. @InitBinder를 통해 해당 컨트롤러에만 영향을 준다.
    - @Validated는 검증기를 실행하라는 어노테이션이다. 이 어노테이션은 WebDataBinder에 등록한 검증기를 찾아서 실행한다. 그런데, 여러 검증기를 등록한다면 그 중에 어떤 검증기가 실행되어야 하는지 구분이 필요한데, 이때 supports()가 사용된다.

<br>

## Controller

```
package hello.itemservice.web.validation;

import hello.itemservice.domain.item.Item;
import hello.itemservice.domain.item.ItemRepository;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.util.StringUtils;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.ObjectError;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import java.util.List;

@Slf4j
@Controller
@RequestMapping("/validation/v2/items")
@RequiredArgsConstructor
public class ValidationItemControllerV2 {

	// 생성자 주입, @RequiredArgsConstructor
    private final ItemRepository itemRepository;
    private final ItemValidator itemValidator;

    // 이 컨트롤러에 요청이 올 때마다 적용된다. 사용하고자 하는 메서드에 @Validated를 넣어줘야한다. addItemV6에서 사용
    @InitBinder
    public void init(WebDataBinder dataBinder) {
        dataBinder.addValidators(itemValidator);
    }

    
    @GetMapping
    public String items(Model model) {
        List<Item> items = itemRepository.findAll();
        model.addAttribute("items", items);
        return "validation/v2/items";
    }

    @GetMapping("/{itemId}")
    public String item(@PathVariable long itemId, Model model) {
        Item item = itemRepository.findById(itemId);
        model.addAttribute("item", item);
        return "validation/v2/item";
    }

    @GetMapping("/add")
    public String addForm(Model model) {
        model.addAttribute("item", new Item());
        return "validation/v2/addForm";
    }

    
    // item 값이 안넘어간다.
//    @PostMapping("/add")
    public String addItemV1(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {
    	
    	// bindingResult가 errors 역할을 해준다.
    	// field에 대한 변수면 new FieldError, 변수와 상관없는 값이라면 new ObjectError
    	//  ModelAttribute처럼 자동으로 Model에 담아준다. item.itemName ="상품이림은 필수입니다." 이런식
    	
    	
        //검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
        	// bindingResult.addError(new FieldError("object name", "field", default message));
            bindingResult.addError(new FieldError("item", "itemName", "상품 이름은 필수 입니다."));
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.addError(new FieldError("item", "price", "가격은 1,000 ~ 1,000,000 까지 허용합니다."));
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", "수량은 최대 9,999 까지 허용합니다."));
        }

        //특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.addError(new ObjectError("item", "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
            }
        }

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
        	log.info("여기여기errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
//    @PostMapping("/add")
    public String addItemV2(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

    	// rejectedValue : 사용자가 입력한 값(거절된 값)
    	// bindingFailure : 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분 값 / 타입오류라면 true?
    	// codes : 메시지 코드, String[]
    	// arguments : 메시지에서 사용하는 인자, Object[]
    	
        //검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
        	//bindingResult.addError(new FieldError("object name", "field", rejected value, bindingFauiure, codes, args,default message));
            bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, null, null, "상품 이름은 필수 입니다."));
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, null, null, "가격은 1,000 ~ 1,000,000 까지 허용합니다."));
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, null ,null, "수량은 최대 9,999 까지 허용합니다."));
        }

        //특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
            	//bindingResult.addError(new ObjectError("object name", codes, args, default message));
                bindingResult.addError(new ObjectError("item",null ,null, "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
            }
        }

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
//    @PostMapping("/add")
    public String addItemV3(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

    	// rejectedValue : 사용자가 입력한 값(거절된 값)
    	// bindingFailure : 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분 값
    	// codes : 메시지 코드, String[]
    	// arguments : 메시지에서 사용하는 인자, Object[]
    	
        //검증 로직
        if (!StringUtils.hasText(item.getItemName())) {
        	// bindingResult.addError(new FieldError("object name", "field", rejected value, bindingFauiure, codes, args,default message));
            bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, new String[]{"required.item.itemName"}, null, null));
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, new String[]{"range.item.price"}, new Object[]{1000, 1000000}, null));
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, new String[]{"max.item.quantity"} ,new Object[]{9999}, null));
        }

        //특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
            	// bindingResult.addError(new ObjectError("object name", codes, args "message"));
                bindingResult.addError(new ObjectError("item",new String[]{"totalPriceMin"} ,new Object[]{10000, resultPrice}, null));
            }
        }

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
//    @PostMapping("/add")
    public String addItemV4(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {
    	
    	// BindingResult 가 제공하는 rejectValue() , reject() 를 사용하면 FieldError , ObjectError 를 직접 생성하지 않고, 깔끔하게 검증 오류를 다룰 수 있다.
    	// errorCode : messages.propeties에서 code.ObjectName.fieldName 인 애를 가져온드.
    	
    	
        log.info("objectName={}", bindingResult.getObjectName());
        log.info("target={}", bindingResult.getTarget());

        // 아래와 기능이 같지만 간단한 공백이나 그런 것들만 그거된다.
//        ValidationUtils.rejectIfEmptyOrWhitespace(bindingResult, "itemName", "required");
        
        if (!StringUtils.hasText(item.getItemName())) {
//        	bindingResult.rejectValue(field, errorCode, args, default message);
            bindingResult.rejectValue("itemName", "required");
        }
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            bindingResult.rejectValue("price", "range", new Object[]{1000, 10000000}, null);
        }
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            bindingResult.rejectValue("quantity", "max", new Object[]{9999}, null);
        }

        //특정 필드가 아닌 복합 룰 검증
        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
//            	bindingResult.reject(errorCode, args, default message);
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
    
//    @PostMapping("/add")
    public String addItemV5(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

        itemValidator.validate(item, bindingResult);

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
    @PostMapping("/add")
    public String addItemV6(@Validated @ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v2/addForm";
        }

        //성공 로직
        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v2/items/{itemId}";
    }

    
    @GetMapping("/{itemId}/edit")
    public String editForm(@PathVariable Long itemId, Model model) {
        Item item = itemRepository.findById(itemId);
        model.addAttribute("item", item);
        return "validation/v2/editForm";
    }

    
    @PostMapping("/{itemId}/edit")
    public String edit(@PathVariable Long itemId, @ModelAttribute Item item) {
        itemRepository.update(itemId, item);
        return "redirect:/validation/v2/items/{itemId}";
    }

}


```

<br>

# Bean Validation
- 검증을 매번 코드로 작성하는 것은 번거롭기 때문에 간단한 어노테이션을 통해 유효성을 검증할 수 있다.
- Bean Validation은 특정한 구현체가 아니라 Bean Validation 2.0(JSR-380)이라는 기술 표준이다. 
- 쉽게 이야기해서 검증 애노테이션과 여러 인터페이스의 모음이다. 마치 JPA가 표준 기술이고 그 구현체로 하이버네이트가 있는 것과 같다.
- Bean Validation을 구현한 기술중에 일반적으로 사용하는 구현체는 하이버네이트 Validator이다. 이름이 하이버네이트가 붙어서 그렇지 ORM과는 관련이 없다

## @Valid 종류(@Validated도 동일하게 사용할 수 있다.)
```
JSR-303 어노테이션 종류

@AssertFalse : false 값만 통과 가능

@AssertTrue : true 값만 통과 가능

@DecimalMax(value=) : 지정된 값 이하의 실수만 통과 가능

@DecimalMin(value=) : 지정된 값 이상의 실수만 통과 가능

@Digits(integer=,fraction=) : 대상 수가 지정된 정수와 소수 자리수보다 적을 경우 통과 가능

@Future : 대상 날짜가 현재보다 미래일 경우만 통과 가능

@Past : 대상 날짜가 현재보다 과거일 경우만 통과 가능

@Max(value) : 지정된 값보다 아래일 경우만 통과 가능

@Min(value) : 지정된 값보다 이상일 경우만 통과 가능

@NotNull : null 값이 아닐 경우만 통과 가능

@Null : null일 겨우만 통과 가능

@Pattern(regex=, flag=) : 해당 정규식을 만족할 경우만 통과 가능

@Size(min=, max=) : 문자열 또는 배열이 지정된 값 사이일 경우 통과 가능

@Valid : 대상 객체의 확인 조건을 만족할 경우 통과 가능

@NotEmpty	주입된 값의 길이가 0이면 오류 발생, 공백도 글자로 인식합니다.

@NotBlank	주입된 값의 공백을 제거하고 길이가 0이면 오류 발생

@Positive	양수가 아니라면 오류 발생

@PositiveOrZero	0또는 양수가 아니라면 오류 발생

@Negative	음수가 아니라면 오류 발생

@NegativeOrZero	0또는 음수가 아니라면 오류 발생

@Email	이메일 형식이 아니라면 오류 발생, 중간에 @가 있는지 정도만 확인한다.

```

<br>

## Spring MVC에서 Bean Validator 사용
- Spring Boot가 ```spring-boot-starter-validation``` 라이브러리를 넣으면 자동으로 Bean Validator를 인지하고 스프링에 통합한다.
- @Validated는 스프링 전용 검증 어노테이션이고, @Valid는 자바 표준 검증 어노테이션이다. 
- 검증시 @Validated, @Valid 둘 다 사용가능하지만 javax.validation.@Valid를 사용하려면 build.gradle에 의존관계를 추가해야 한다. ```implementation 'org.springframework.boot:spring-boot-starter-validation'```
- Bean Validation은 바인딩에 성공한 필드여야 적용이 된다. 쉽게 생각해보면 일단 모델 객체에 바인딩 받는 값이 정상으로 들어와야 검증하는 의미가 있기 때문이다.

<br>

## Bean Validator 그룹 사용하기
- 사실 사용법은 간단하다. 마커 인터페이스를 설정해 주는 것이다.
- ```@어노테이션(groups = 인터페이스이름.class or {인터페이스1.class, ...2.class, ...})``` 처럼 사용하면 된다.

<br>

###  마커 인터페이스
![image](https://user-images.githubusercontent.com/74396651/182607889-7d849397-ec16-437f-8288-804cdfa5d0ab.png)

### DTO
![image](https://user-images.githubusercontent.com/74396651/182607854-4e578509-5fd7-47f6-a934-f4469354aab5.png)

### Controller
![image](https://user-images.githubusercontent.com/74396651/182608215-70381842-5610-419c-8edc-3b53b7974d64.png)


![image](https://user-images.githubusercontent.com/74396651/182608263-fa69ee6f-9e37-4581-8b12-4830ba261c34.png)

<br>

## 전송 객체 분리하기
- 사실 위처럼 마커 인터페이스를 통해 groups로 묶는 것은 로직이 길어질 수록 점점 복잡해지고 검증이 중복될 수도 있다.
- 따라서 전송 객체를 분리하여 각각의 도매인 객체에 Validation을 해준다면 유지보수에도 좋고 확실하게 판단할 수 있게 된다!

<br>

## 코드로 살펴보기
- 도메인 객체 Item
- add를 통해 들어오는 ItemSaveForm 객체
- edit을 통해 들어오는 ItemUpdateForm 객체 로 나누어 각각 사용용도에 맞게 검증한다.

#### item
```
@Data
public class Item {
 private Long id;
 private String itemName;
 private Integer price;
 private Integer quantity;
}
```

<br>

#### ItemSaveForm
```
package hello.itemservice.web.validation.form;
import lombok.Data;
import org.hibernate.validator.constraints.Range;
import javax.validation.constraints.Max;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;
@Data
public class ItemSaveForm {
 @NotBlank
 private String itemName;
 @NotNull
 @Range(min = 1000, max = 1000000)
 private Integer price;
 @NotNull
 @Max(value = 9999)
 private Integer quantity;
}
```

<br>

#### ItemUpdateForm
```
package hello.itemservice.web.validation.form;
import lombok.Data;
import org.hibernate.validator.constraints.Range;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;
@Data
public class ItemUpdateForm {
 @NotNull
 private Long id;
 @NotBlank
 private String itemName;
 @NotNull
 @Range(min = 1000, max = 1000000)
 private Integer price;
 //수정에서는 수량은 자유롭게 변경할 수 있다.
 private Integer quantity;
}
```

<br>

### Controller
```
package hello.itemservice.web.validation;

import hello.itemservice.domain.item.Item;
import hello.itemservice.domain.item.ItemRepository;
import hello.itemservice.domain.item.SaveCheck;
import hello.itemservice.domain.item.UpdateCheck;
import hello.itemservice.web.validation.form.ItemSaveForm;
import hello.itemservice.web.validation.form.ItemUpdateForm;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import java.util.List;

@Slf4j
@Controller
@RequestMapping("/validation/v4/items")
@RequiredArgsConstructor
public class ValidationItemControllerV4 {

    private final ItemRepository itemRepository;

    @GetMapping
    public String items(Model model) {
        List<Item> items = itemRepository.findAll();
        model.addAttribute("items", items);
        return "validation/v4/items";
    }

    @GetMapping("/{itemId}")
    public String item(@PathVariable long itemId, Model model) {
        Item item = itemRepository.findById(itemId);
        model.addAttribute("item", item);
        return "validation/v4/item";
    }

    @GetMapping("/add")
    public String addForm(Model model) {
        model.addAttribute("item", new Item());
        return "validation/v4/addForm";
    }

    // ItemSaveForm 객체가 들어와 Item 객체로 변환된다.
    @PostMapping("/add")
    public String addItem(@Validated @ModelAttribute("item") ItemSaveForm form, BindingResult bindingResult, RedirectAttributes redirectAttributes) {

        //특정 필드가 아닌 복합 룰 검증
        if (form.getPrice() != null && form.getQuantity() != null) {
            int resultPrice = form.getPrice() * form.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

        //검증에 실패하면 다시 입력 폼으로
        if (bindingResult.hasErrors()) {
            log.info("errors={} ", bindingResult);
            return "validation/v4/addForm";
        }

        //성공 로직
        Item item = new Item();
        item.setItemName(form.getItemName());
        item.setPrice(form.getPrice());
        item.setQuantity(form.getQuantity());

        Item savedItem = itemRepository.save(item);
        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/validation/v4/items/{itemId}";
    }

    @GetMapping("/{itemId}/edit")
    public String editForm(@PathVariable Long itemId, Model model) {
        Item item = itemRepository.findById(itemId);
        model.addAttribute("item", item);
        return "validation/v4/editForm";
    }

    // ItemUpdateForm 객체가 들어와 Item 객체로 변환된다.
    @PostMapping("/{itemId}/edit")
    public String edit(@PathVariable Long itemId, @Validated @ModelAttribute("item") ItemUpdateForm form, BindingResult bindingResult) {

        //특정 필드가 아닌 복합 룰 검증
        if (form.getPrice() != null && form.getQuantity() != null) {
            int resultPrice = form.getPrice() * form.getQuantity();
            if (resultPrice < 10000) {
                bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
            }
        }

        if (bindingResult.hasErrors()) {
            log.info("errors={}", bindingResult);
            return "validation/v4/editForm";
        }

        Item itemParam = new Item();
        itemParam.setItemName(form.getItemName());
        itemParam.setPrice(form.getPrice());
        itemParam.setQuantity(form.getQuantity());

        itemRepository.update(itemId, itemParam);
        return "redirect:/validation/v4/items/{itemId}";
    }

}


```
	
	










<br>
<br>
<br>
<br>
<br>


참조 : 인프런 김영한님 강의


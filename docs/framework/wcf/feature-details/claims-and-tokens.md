---
title: "Утверждения и маркеры | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "утверждения [WCF], и маркеры"
ms.assetid: eff167f3-33f8-483d-a950-aa3e9f97a189
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# Утверждения и маркеры
В этом разделе описываются различные типы утверждений, создаваемые [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] из поддерживаемых маркеров по умолчанию.  
  
 Анализировать утверждения учетных данных клиента можно с помощью классов <xref:System.IdentityModel.Claims.ClaimSet> и <xref:System.IdentityModel.Claims.Claim>.  Класс `ClaimSet` содержит коллекцию объектов `Claim`.  Каждый объект `Claim` имеет следующие важные члены.  
  
-   Свойство <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> возвращает универсальный код ресурса \(URI\), который указывает тип делаемого утверждения.  Например, тип утверждения может представлять собой отпечаток сертификата; в этом случае URI будет следующим: http:schemas.microsoft.com\/ws\/20005\/05\/identity\/claims\/thumprint.  
  
-   Свойство <xref:System.IdentityModel.Claims.Claim.Right%2A> возвращает универсальный код ресурса \(URI\), который указывает право утверждения.  Предопределенные права находятся в классе <xref:System.IdentityModel.Claims.Rights> \(<xref:System.IdentityModel.Claims.Rights.Identity%2A>, <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A>\).  
  
-   Свойство <xref:System.IdentityModel.Claims.Claim.Resource%2A> возвращает ресурс, связанный с утверждением.  
  
 Каждый объект <xref:System.IdentityModel.Claims.ClaimSet> также имеет свойство <xref:System.IdentityModel.Claims.ClaimSet.Issuer%2A>, которое представляет <xref:System.IdentityModel.Claims.ClaimSet> издателя \(`Issuer`\).  
  
## Учетные записи Windows  
 Когда учетные данные клиента сопоставляются с учетной записью пользователя Windows, полученный в результате объект <xref:System.IdentityModel.Claims.ClaimSet> имеет следующие значения.  
  
-   `Issuer` \- значение, возвращенное статическим свойством Windows класса <xref:System.IdentityModel.Claims.ClaimSet>.  
  
-   Коллекция содержит следующие утверждения.  
  
    -   Объект <xref:System.IdentityModel.Claims.Claim>, у которого свойство <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> указывает на идентификатор безопасности \(SID\), свойство <xref:System.IdentityModel.Claims.Claim.Right%2A> имеет значение `Identity`, а свойство <xref:System.IdentityModel.Claims.Claim.Resource%2A> возвращает фактическое значение идентификатора безопасности.  Идентификатор безопасности \(SID\) \- это уникальное значение, выдаваемое контроллером домена каждому пользователю.  Идентификатор безопасности используется для идентификации пользователя при взаимодействии с системой безопасности Windows.  
  
    -   Объект <xref:System.IdentityModel.Claims.Claim>, у которого свойство <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> указывает на идентификатор безопасности, свойство <xref:System.IdentityModel.Claims.Claim.Right%2A> имеет значение `PossessProperty`, а свойство <xref:System.IdentityModel.Claims.Claim.Resource%2A> представляет собой значение идентификатора безопасности.  
  
    -   Объект <xref:System.IdentityModel.Claims.Claim>, у которого свойство <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> имеет значение <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A>, свойство <xref:System.IdentityModel.Claims.Claim.Right%2A> имеет значение `PossessProperty`, а свойство <xref:System.IdentityModel.Claims.Claim.Resource%2A> представляет собой строку, содержащую имя пользователя \(например, "MYMACHINE\\Bob"\).  
  
    -   Дополнительные утверждения типа "идентификатор безопасности" со значением <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> для различных групп, к которым принадлежит пользователь.  
  
## Сертификаты  
 Когда учетные данные клиента представляют собой сертификат, полученный в результате объект <xref:System.IdentityModel.Claims.ClaimSet> имеет следующие значения.  
  
-   В случае самостоятельно выданного сертификата издателем \(`Issuer`\) является сам объект <xref:System.IdentityModel.Claims.ClaimSet>.  Объект <xref:System.IdentityModel.Claims.ClaimSet> возвращает свойство <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> со значением <xref:System.IdentityModel.Claims.ClaimTypes.Thumbprint%2A>, свойство <xref:System.IdentityModel.Claims.Claim.Right%2A> со значением `Identity` и свойство <xref:System.IdentityModel.Claims.Claim.Resource%2A> со значением, представляющим собой массив типа <xref:System.Byte>, который содержит отпечаток сертификата.  
  
-   В случае сертификата, выданного центром сертификации, издателем является `ClaimSet`, представляющий сертификат центра.  
  
-   В коллекцию входят следующие утверждения \(`Claims`\).  
  
    -   Объект `Claim`, у которого свойство `ClaimType` указывает на отпечаток, свойство `Right` имеет значение PossessProperty, а значение свойства `Resource` представляет собой байтовый массив, содержащий отпечаток сертификата.  
  
    -   Дополнительные утверждения с правом "PossessProperty" различных типов, включая "X500DistinguishedName","Dns", "Name", "Upn" и "Rsa", представляют различные свойства сертификата.  Ресурс для утверждения типа Rsa представляет собой открытый ключ, связанный с сертификатом. **Примечание**: в случаях, когда тип учетных данных клиента \- сертификат, сопоставляемый с учетной записью Windows, формируется два объекта `ClaimSet`.  Первый объект содержит все утверждения, связанные с учетной записью Windows, а второй содержит все утверждения, связанные с сертификатом.  
  
## Имя пользователя\/пароль  
 Когда учетные данные клиента представляют собой имя пользователя\/пароль \(или эквивалент\), которые не сопоставляются с учетной записью Windows, полученный в результате объект `ClaimSet` будет выдан статическим свойством <xref:System.IdentityModel.Claims.ClaimSet.System%2A> класса `ClaimSet`.  Объект `ClaimSet` содержит утверждение с правом `Identity` типа  `` <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A>, ресурсом которого является предоставленное клиентом имя пользователя.  У соответствующего утверждения свойство`Right` имеет значение `PossessProperty`.  
  
## Ключи RSA  
 При использовании ключа RSA, не связанного с сертификатом, полученный в результате объект `ClaimSet` является самостоятельно выданным и содержит утверждение с правом `Identity`  `` типа `` <xref:System.IdentityModel.Claims.ClaimTypes.Rsa%2A>, ресурсом которого является ключ RSA.  У соответствующего утверждения свойство `Right` имеет значение `PossessProperty`.  
  
## Маркеры SAML  
 Когда для проверки подлинности клиента используется маркер на языке SAML, полученный в результате объект `ClaimSet` будет выдан сущностью, подписавшей маркер SAML; зачастую это сертификат службы маркеров безопасности, которая выдала маркер SAML.  Объект `ClaimSet` содержит различные утверждения, обнаруженные в маркере SAML.  Если маркер SAML содержит свойство `SamlSubject` с именем, отличным от `null`, создается утверждение с правом `Identity`, типом <xref:System.IdentityModel.Claims.ClaimTypes.NameIdentifier%2A> и ресурсом типа <xref:System.IdentityModel.Tokens.SamlNameIdentifierClaimResource>.  
  
## Идентификационные утверждения и ServiceSecurityContext.IsAnonymous  
 Если ни один из объектов `ClaimSet`, полученных из учетных данных клиента, не содержит утверждения, у которого свойство `Right` имеет значение `Identity,` свойство <xref:System.ServiceModel.ServiceSecurityContext.IsAnonymous%2A> возвращает значение `true`.  При наличии одного или нескольких таких утверждений свойство `IsAnonymous` возвращает значение `false`.  
  
## См. также  
 <xref:System.IdentityModel.Claims.ClaimSet>   
 <xref:System.IdentityModel.Claims.Claim>   
 <xref:System.IdentityModel.Claims.Rights>   
 <xref:System.IdentityModel.Claims.ClaimTypes>   
 [Управление утверждениями и авторизацией с помощью модели удостоверения](../../../../docs/framework/wcf/feature-details/managing-claims-and-authorization-with-the-identity-model.md)
# Issue a credential

Issuer通过owner提供的subject，type数据生成凭证。根据Issuer和owner的关系，凭证可以分为自声明凭证和第三方凭证，前者的Issuer就是owner，就是自己给自己颁发凭证，主要封装有个人提供的信息；后者的Issuer是除owner以外的其他DID，就是他人（如可信机构）根据owner提供给的数据封装凭证。

凭证是一次生成，不可修改。

## Example

自声明凭证示例：
```c
//doc is holder's document.
time_t expires = DIDDocument_GetExpires(doc);
DID *did = DIDDocumen_GetSubject(doc);

//create Issuer with Default Key
Issuer *issuer = Issuer_Create(did, NULL, store);
DIDURL *credid = DIDURL_NewFromDid(did, "mycredential");
const char *types[] = {"BasicProfileCredential", "PhoneCredential",
                       "SelfProclaimedCredential"};
Property props[7];
props[0].key = "name";
props[0].value = "John";
props[1].key = "gender";
props[1].value = "Male";
props[2].key = "nation";
props[2].value = "Singapore";
props[3].key = "language";
props[3].value = "English";
props[4].key = "email";
props[4].value = "john@example.com";
props[5].key = "twitter";
props[5].value = "@john";
props[6].key = "phone";
props[6].value = "132780456";

Credential *vc = Issuer_CreateCredential(issuer, did, credid, types, 3, props, 7, expires, storepass);
//destroy 'issuer' if 'issuer' will not to be used after!!
Issuer_Destroy(issuer);
//destroy 'credid' if 'credid' will not to be used after!!
DIDURL_Destroy(credid);
... ... ... ...
//destroy 'vc' if 'vc' will not to be used after!!
Credential_Destroy(vc);
```
第三方凭证示例：
```c
time_t expires = DIDDocument_GetExpires(holderDoc);
DID *holder = DIDDocument_GetSubject(holderDoc);

Issuer *issuer = Issuer_Create(issuerid, NULL, store);
DIDURL *credid = DIDURL_NewFromDid(holder, "basecredential");

const char *types[] = {"BasicProfileCredential", "SelfProclaimedCredential"};
const char *propdata = "{\"name\":\"Jay Holtslander\",\"alternateName\":\"Jason Holtslander\",\"booleanValue\":true,\"numberValue\":1234,\"doubleValue\":9.5,\"nationality\":\"Canadian\",\"birthPlace\":{\"type\":\"Place\",\"address\":{\"type\":\"PostalAddress\",\"addressLocality\":\"Vancouver\",\"addressRegion\":\"BC\",\"addressCountry\":\"Canada\"}},\"affiliation\":[{\"type\":\"Organization\",\"name\":\"Futurpreneur\",\"sameAs\":[\"https://twitter.com/futurpreneur\",\"https://www.facebook.com/futurpreneur/\",\"https://www.linkedin.com/company-beta/100369/\",\"https://www.youtube.com/user/CYBF\"]}],\"alumniOf\":[{\"type\":\"CollegeOrUniversity\",\"name\":\"Vancouver Film School\",\"sameAs\":\"https://en.wikipedia.org/wiki/Vancouver_Film_School\",\"year\":2000},{\"type\":\"CollegeOrUniversity\",\"name\":\"CodeCore Bootcamp\"}],\"gender\":\"Male\",\"Description\":\"Technologist\",\"disambiguatingDescription\":\"Co-founder of CodeCore Bootcamp\",\"jobTitle\":\"Technical Director\",\"worksFor\":[{\"type\":\"Organization\",\"name\":\"Skunkworks Creative Group Inc.\",\"sameAs\":[\"https://twitter.com/skunkworks_ca\",\"https://www.facebook.com/skunkworks.ca\",\"https://www.linkedin.com/company/skunkworks-creative-group-inc-\",\"https://plus.google.com/+SkunkworksCa\"]}],\"url\":\"https://jay.holtslander.ca\",\"image\":\"https://s.gravatar.com/avatar/961997eb7fd5c22b3e12fb3c8ca14e11?s=512&r=g\",\"address\":{\"type\":\"PostalAddress\",\"addressLocality\":\"Vancouver\",\"addressRegion\":\"BC\",\"addressCountry\":\"Canada\"},\"sameAs\":[\"https://twitter.com/j_holtslander\",\"https://pinterest.com/j_holtslander\",\"https://instagram.com/j_holtslander\",\"https://www.facebook.com/jay.holtslander\",\"https://ca.linkedin.com/in/holtslander/en\",\"https://plus.google.com/+JayHoltslander\",\"https://www.youtube.com/user/jasonh1234\",\"https://github.com/JayHoltslander\",\"https://profiles.wordpress.org/jasonh1234\",\"https://angel.co/j_holtslander\",\"https://www.foursquare.com/user/184843\",\"https://jholtslander.yelp.ca\",\"https://codepen.io/j_holtslander/\",\"https://stackoverflow.com/users/751570/jay\",\"https://dribbble.com/j_holtslander\",\"http://jasonh1234.deviantart.com/\",\"https://www.behance.net/j_holtslander\",\"https://www.flickr.com/people/jasonh1234/\",\"https://medium.com/@j_holtslander\"]}";

Credential *vc = Issuer_CreateCredentialByString(issuer, holder, credid, types, 2,
                                                 propdata, expires, storepass);
//destroy 'issuer' if 'issuer' will not to be used after!!
Issuer_Destroy(issuer);
//destroy 'credid' if 'credid' will not to be used after!!
DIDURL_Destroy(credid);
... ... ... ...
//destroy 'vc' if 'vc' will not to be used after!!
Credential_Destroy(vc);
```

## Usage

```c
Issuer *Issuer_Create(DID *did, DIDURL *signkey, DIDStore *store);
```
该方法用来生成Issuer object。`did`就是指定作为Issuer的DID，`signkey`是did作为Issuer用来签名的Authentication Key，若signkey为NULL，则默认使用Default Key。

```c
void Issuer_Destroy(Issuer *issuer);
```

用完Issuer object，需要销毁对象。

```c
typedef struct Property {
    char *key;
    char *value;
} Property;
  
Credential *Issuer_CreateCredential(Issuer *issuer, DID *holder, DIDURL *credid,
        const char **types, size_t typesize, Property *subject, int size,
        time_t expires, const char *storepass);
```
Issuer根据提供的subject object颁发凭证。该接口的所有参数均不可为空，具体使用可参考sample。

```c
Credential *Issuer_CreateCredentialByString(Issuer *issuer, DID *holder,
        DIDURL *credid, const char **types, size_t typesize, const char *subject,
        time_t expires, const char *storepass);
```
Issuer根据提供的subject string颁发凭证。该接口的所有参数均不可为空，具体使用可参考sample。

# Issue a Credential

The Issuer generates credentials through the subject and type data provided by the owner. According to the relationship between the issuer and owner, credentials can be divided into self-declared credentials and third-party credentials. The Issuer of the former is the owner, that is, the credential issued by itself is mainly packaged with the information provided by individuals. The Issuer of the latter is the other DIDs except for the owner, or the data encapsulation credential provided by others (such as a trusted agency) according to the owner.

Credential is generated at one time and cannot be modified.

## Example

Example of self-declaration credential:

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

Example of third-party credential:

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

This method is used to generate Issuer object. did is the DID designated as the Issuer; signkey is the Authentication Key used by did as the Issuer to sign. If signkey is NULL, the Default Key is used by default.

```c
void Issuer_Destroy(Issuer *issuer);
```

The Issuer object needs to be destroyed after being used.

```c
typedef struct Property {
    char *key;
    char *value;
} Property;
  
Credential *Issuer_CreateCredential(
        Issuer *issuer,
        DID *holder,
        DIDURL *credid,
        const char **types, size_t typesize,
        Property *subject, int size,
        time_t expires,
        const char *storepass);
```

The Issuer issues credentials according to the provided subject object. All parameters of this interface cannot be null - refer to sample for specific use.

```c
Credential *Issuer_CreateCredentialByString(
        Issuer *issuer,
        DID *holder,
        DIDURL *credid,
        const char **types, size_t typesize,
        const char *subject,
        time_t expires, 
        const char *storepass);
```

The Issuer issues credentials according to the provided subject string. All parameters of this interface cannot be null - refer to sample for specific use.

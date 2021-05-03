# ICT120bscs3b
# Importing modules

import re
from nltk.corpus import wordnet

# Building a list of Keywords
import nltk
nltk.download('wordnet')

list_words=['hello','timings','check','lost','claim']
#added keyword are check,lost, and claim



list_syn={}
for word in list_words:
    synonyms=[]
    for syn in wordnet.synsets(word):
        for lem in syn.lemmas():
            
            # Remove any special characters from synonym strings
            lem_name = re.sub('[^a-zA-Z0-9 \\]', ' ', lem.name())
            synonyms.append(lem_name)
   
    list_syn[word]=set(synonyms)
    
print (list_syn)




# Building dictionary of Intents & Keywords
keywords={}
keywords_dict={}

# Defining a new key in the keywords dictionary
keywords['greet']=[]

# Populating the values in the keywords dictionary with synonyms of keywords formatted with RegEx metacharacters 
for synonym in list(list_syn['hello']):
    keywords['greet'].append('.*\b'+synonym+'\b.*')

# Defining a new key in the keywords dictionary
keywords['timings']=[]

# Populating the values in the keywords dictionary with synonyms of keywords formatted with RegEx metacharacters 
for synonym in list(list_syn['timings']):
    keywords['timings'].append('.*\b'+synonym+'\b.*')



# Added new keywords
keywords['check']=[]

# Populating the values in the keywords dictionary with synonyms of keywords formatted with RegEx metacharacters 
for synonym in list(list_syn['check']):
    keywords['check'].append('.*\b'+synonym+'\b.*')

keywords['lost']=[]

# Populating the values in the keywords dictionary with synonyms of keywords formatted with RegEx metacharacters 
for synonym in list(list_syn['lost']):
    keywords['lost'].append('.*\b'+synonym+'\b.*')
    


    keywords['claim_ATM']=[]

# Populating the values in the keywords dictionary with synonyms of keywords formatted with RegEx metacharacters 
for synonym in list(list_syn['claim']):
    keywords['claim_ATM'].append('.*\b'+synonym+'\b.*')

for intent, keys in keywords.items():
    
    # Joining the values in the keywords dictionary with the OR (|) operator updating them in keywords_dict dictionary
    keywords_dict[intent]=re.compile('|'.join(keys))
print (keywords_dict)




# Building a dictionary of responses
responses={
    'greet':'Hello! How can I help you?',
    'timings':'We are open from 9AM to 5PM, Monday to Friday. We are closed on weekends and public holidays.',
    'ask': 'How can I open a bank account?.',
    'fallback':'I dont quite understand. Could you repeat that?',

#ADDED responses   
    'check':'You can check your account balance on our website, on our mobile banking app or you check it on your local electronic banking outlet (Automated Teller Machine)',
    'lost': 'You can  report it to the  branch where you opened the ATM account. It is  very important to report the incident as quickly as possible in order to avoid unauthorized use of your ATM card. In other words, to prevent someone from accessing your ATM account.',
    'claim_ATM': 'Your new ATM card will be ready for claiming within 3 to 5 business days'
}

print ("Welcome to MyBank. How may I help you?")

# While loop to run the chatbot indefinetely
while (True):  
    
    # Takes the user input and converts all characters to lowercase
    user_input = input().lower()
    
    # Defining the Chatbot's exit condition
    if user_input == 'quit': 
        print ("Thank you for visiting.")
        break    
    
    matched_intent = None
    
    for intent,pattern in keywords_dict.items():
        
        # Using the regular expression search function to look for keywords in user input
        if re.search(pattern, user_input): 
            
            # if a keyword matches, select the corresponding intent from the keywords_dict dictionary
            matched_intent=intent  
    
    # The fallback intent is selected by default
    key='fallback'
    if matched_intent in responses:
        
        # If a keyword matches, the fallback intent is replaced by the matched intent as the key for the responses dictionary
        key = matched_intent 
    
    # The chatbot prints the response that matches the selected intent
    print (responses[key])

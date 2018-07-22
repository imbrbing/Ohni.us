import json
from configparser import ConfigParser
import ssl
import urllib.request
import sys
from utils import *
from web3 import Web3, HTTPProvider
import http.client

def is_json(myjson):
  with open(myjson) as f:
      print ("VALIDATING JSON %s" % myjson)
      try:
          json_object = json.load(f)
      except ValueError as e:
          print('INVALID JSON: %s' % e)
          return False
      return json_object

def validate_eth_address(eth_addr):
     E_address = eth_addr
     E_address.strip()
     print("VALIDATING ETHEREUM ADDRESS:  %s" % E_address)
     w3 = Web3(HTTPProvider('https://api.myetherapi.com/eth'))
     if w3.isChecksumAddress(E_address):
        print("Valid checksum")
     else:
        print ("Invalid checksum, converting to valid checksum") 
        E_address = Web3.toChecksumAddress(E_address)
        print (E_address) 
     try:
         eth_balance = Web3.fromWei(w3.eth.getBalance(E_address), 'ether')
         print("Balance = :  %s" % eth_balance)
     except ValueError as e:
         print('\t INVALID ADDRESS: %s' % e) 

def validate_definition(description):
     print ("VALIDATING DEFINITION: %s " % description)
     length = len(description)
     if (length  > 1000):
        print("error, string length is %s" % len(description))    
        return False

def validate_links(links):
     ssl._create_default_https_context = ssl._create_unverified_context
     print ("VALIDATING  LINKS:")
     for k, v  in links.items():
        req = urllib.request.Request(v)
        try: 
              if urllib.request.urlopen(req):
                   print (v,  ": IS OK")
        except urllib.error.URLError as e:
              print(v, e.reason)   
             
        #print(v)

def main():
  if len(sys.argv) == 2:
     result = is_json(sys.argv[1])

     if (result):
        try:
             parser = ConfigParser()
             parser.read('./Coin.cfg')
        except parser.ParsingError as err:
             print('Could not parse:', err)

        for section_name in parser.sections():
             print('Section:', section_name)
             print('  Options:', parser.options(section_name))
             for name, value in parser.items(section_name):
                 print('  {} = {}'.format(name, value))
             print()

        for k, v in result.items():
             print (v)
          #if 'address' in result:
             #validate_eth_address(result['address'])
          #else:
             #print ("No Address found")
          #if 'description' in result:
             #validate_definition(result['description'])
          #else:
             #print ("\t No Description found")
          #if 'links' in result:
             #validate_links(result['links'])
          #else:
             #print("No links found")
     else:
          print("##############################")
          print("# Json check FAILED")
          print("##############################")   
  else:
      print("Usage: validateJson.py filename.json")

if __name__ == '__main__':
    main()
exit(1)

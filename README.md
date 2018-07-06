# k

def get_last_transactions_2(credit_card_number):    
    ticket_number = generate_ticket_1(credit_card_number)

    input_query = """<XML><MessageType>1200</MessageType><ProcCode>777010</ProcCode><SOAPMessage><soap:Envelope xmlns:soap='http://schemas.xmlsoap.org/soap/envelope/' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance' xmlns:xsd='http://www.w3.org/2001/XMLSchema'><soap:Body><StatementInquiry xmlns='http://CTL.COM.Services.Prime.Issuer.WebServices/PrimeIssuerServices'><xmlRequest><Header><MessageID>1234</MessageID><CorrelationID></CorrelationID><SystemID></SystemID><RequestorID></RequestorID><Ticket>"""+ticket_number+"""</Ticket><CallerRef></CallerRef><Origin></Origin><Culture></Culture></Header><Paging><Size>0</Size><Key></Key></Paging><Period>Current</Period><DateFrom></DateFrom><DateTo></DateTo><Content>Account</Content><Reference>C</Reference><Number>"""+credit_card_number+"""</Number><HierarchicalTrxns>false</HierarchicalTrxns><CustomerNumber></CustomerNumber><LevelDepth>0</LevelDepth><TransactionMemos>None</TransactionMemos></xmlRequest></StatementInquiry></soap:Body></soap:Envelope></SOAPMessage></XML>"""
    response = call_api(input_query)
    last_transactions = parse_last_transactions(response)

    current_transaction = []
    if len(last_transactions) <= 5:
        input_query = """<XML><MessageType>1200</MessageType><ProcCode>777010</ProcCode><SOAPMessage><soap:Envelope xmlns:soap='http://schemas.xmlsoap.org/soap/envelope/' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance' xmlns:xsd='http://www.w3.org/2001/XMLSchema'><soap:Body><StatementInquiry xmlns='http://CTL.COM.Services.Prime.Issuer.WebServices/PrimeIssuerServices'><xmlRequest><Header><MessageID>1234</MessageID><CorrelationID></CorrelationID><SystemID></SystemID><RequestorID></RequestorID><Ticket>"""+ticket_number+"""</Ticket><CallerRef></CallerRef><Origin></Origin><Culture></Culture></Header><Paging><Size>0</Size><Key></Key></Paging><Period>Last</Period><DateFrom></DateFrom><DateTo></DateTo><Content>Account</Content><Reference>C</Reference><Number>"""+credit_card_number+"""</Number><HierarchicalTrxns>false</HierarchicalTrxns><CustomerNumber></CustomerNumber><LevelDepth>0</LevelDepth><TransactionMemos>None</TransactionMemos></xmlRequest></StatementInquiry></soap:Body></soap:Envelope></SOAPMessage></XML>"""
        response = call_api(input_query)
        current_transaction = parse_last_transactions(response)

    return last_transactions + current_transaction

def get_credit_card_details_2(credit_card_number):    
    ticket_number = generate_ticket_1(credit_card_number)

    input_query = """<XML><MessageType>1200</MessageType><ProcCode>777010</ProcCode><SOAPMessage><soap:Envelope xmlns:soap='http://schemas.xmlsoap.org/soap/envelope/' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance' xmlns:xsd='http://www.w3.org/2001/XMLSchema'><soap:Body><StatementInquiry xmlns='http://CTL.COM.Services.Prime.Issuer.WebServices/PrimeIssuerServices'><xmlRequest><Header><MessageID>1234</MessageID><CorrelationID></CorrelationID><SystemID></SystemID><RequestorID></RequestorID><Ticket>"""+ticket_number+"""</Ticket><CallerRef></CallerRef><Origin></Origin><Culture></Culture></Header><Paging><Size>0</Size><Key></Key></Paging><Period>Last</Period><DateFrom></DateFrom><DateTo></DateTo><Content>Account</Content><Reference>C</Reference><Number>"""+credit_card_number+"""</Number><HierarchicalTrxns>false</HierarchicalTrxns><CustomerNumber></CustomerNumber><LevelDepth>0</LevelDepth><TransactionMemos>None</TransactionMemos></xmlRequest></StatementInquiry></soap:Body></soap:Envelope></SOAPMessage></XML>"""
    response = call_api(input_query)
    due_date, due_amount = parse_cc_date_amount(response)
    return (due_date, due_amount)

def generate_ticket_1(credit_card_number):
    input_query = """<XML><MessageType>1200</MessageType><ProcCode>777001</ProcCode><SOAPMessage><soap:Envelope xmlns:soap='http://schemas.xmlsoap.org/soap/envelope/' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance' xmlns:xsd='http://www.w3.org/2001/XMLSchema'><soap:Body><AcquireTicket xmlns='http://CTL.COM.Services.Prime.Issuer.WebServices/PrimeIssuerServices'><xmlRequest><Header><MessageID>1111</MessageID><CorrelationID></CorrelationID><SystemID></SystemID><RequestorID></RequestorID><Ticket>"""+credit_card_number+"""</Ticket><CallerRef></CallerRef><Origin></Origin><Culture></Culture></Header><Ticket><hostIP></hostIP><applicationName></applicationName></Ticket></xmlRequest></AcquireTicket></soap:Body></soap:Envelope></SOAPMessage></XML>"""
    response = call_api(input_query)
    ticket_number = parse_ticket_number(response)
    return ticket_number

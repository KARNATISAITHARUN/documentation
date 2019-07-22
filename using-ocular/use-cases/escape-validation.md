Based on Chetan's Webinar "How To Find Malicious Backdoors and Business Logic Vulnerabilities in Your Code"

# Escaping Validation Frameworks

Malicious user bypasses your standard validation framework by encoding a particular string, pass it through the validation 
function, and return the original string.

The original string is returned, it enters the loop which is the branch condition, and then the decode function happens and the 
malacious query is executed.

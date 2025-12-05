// Parse encoded conversation string "[U]msg1[A]msg2..." into messages array
function parseConversation(encoded) {
  var messages = [];
  var parts = encoded.split(/(\[U\]|\[A\])/);

  var currentRole = null;
  for (var i = 0; i < parts.length; i++) {
    if (parts[i] === '[U]') {
      currentRole = 'user';
    } else if (parts[i] === '[A]') {
      currentRole = 'assistant';
    } else if (parts[i] && parts[i].length > 0 && currentRole) {
      messages.push({
        role: currentRole,
        content: parts[i]
      });
      currentRole = null;
    }
  }

  return messages;
}

// Get response from Gemini API
function getClaudeResponse(messages) {
  var apiKey = localStorage.getItem('api_key');
  var baseUrl = localStorage.getItem('base_url') || 'https://generativelanguage.googleapis.com/v1beta/models/';
  var model = localStorage.getItem('model') || 'gemini-2.5-flash-lite:generateContent';
  var systemMessage = localStorage.getItem('system_message') || "You're running on a Pebble smartwatch. Please respond in plain text without any formatting, keeping your responses within 1-3 sentences.";

  if (!apiKey) {
    console.log('No API key configured');
    // Send error, then end
    Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'No API key configured. Please configure in settings.' });
    Pebble.sendAppMessage({ 'RESPONSE_END': 1 });
    return;
  }

  console.log('Sending request to Gemini API with ' + messages.length + ' messages');

  var xhr = new XMLHttpRequest();
  var url = baseUrl + model + '?key=' + apiKey;
  xhr.open('POST', url, true);
  xhr.setRequestHeader('Content-Type', 'application/json');

  xhr.timeout = 15000;

  xhr.onload = function () {
    if (xhr.status === 200) {
      try {
        var data = JSON.parse(xhr.responseText);

        // Extract text from Gemini response format
        if (data.candidates && data.candidates.length > 0 && 
            data.candidates[0].content && 
            data.candidates[0].content.parts && 
            data.candidates[0].content.parts.length > 0) {
          
          var responseText = '';
          var parts = data.candidates[0].content.parts;
          
          for (var i = 0; i < parts.length; i++) {
            if (parts[i].text) {
              responseText += parts[i].text;
            }
          }

          console.log(JSON.stringify(data.candidates[0].content, null, 2));

          responseText = responseText.trim();

          if (responseText.length > 0) {
            console.log('Sending response: ' + responseText);
            Pebble.sendAppMessage({ 'RESPONSE_TEXT': responseText });
          } else {
            console.log('No text in response');
            Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'No response from Gemini' });
          }
        } else {
          console.log('No content in response');
          Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'No response from Gemini' });
        }
      } catch (e) {
        console.log('Error parsing response: ' + e);
        Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'Error parsing response' });
      }
    } else {
      console.log('API error: ' + xhr.status + ' - ' + xhr.responseText);
      // Parse error response and extract message
      var errorMessage = xhr.responseText;

      try {
        var errorData = JSON.parse(xhr.responseText);
        if (errorData.error && errorData.error.message) {
          errorMessage = errorData.error.message;
        }
      } catch (e) {
        console.log('Failed to parse error response: ' + e);
      }

      // Send error
      Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'Error ' + xhr.status + ': ' + errorMessage });
    }

    // Always send end signal
    Pebble.sendAppMessage({ 'RESPONSE_END': 1 });
  };

  xhr.onerror = function () {
    console.log('Network error');
    Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'Network error occurred' });
    Pebble.sendAppMessage({ 'RESPONSE_END': 1 });
  };

  xhr.ontimeout = function () {
    console.log('Request timeout');
    Pebble.sendAppMessage({ 'RESPONSE_TEXT': 'Request timed out. Likely problems on Google\'s side.' });
    Pebble.sendAppMessage({ 'RESPONSE_END': 1 });
  };

  // Convert messages to Gemini format
  var geminiContents = [];
  
  for (var i = 0; i < messages.length; i++) {
    var message = messages[i];
    geminiContents.push({
      role: message.role === 'assistant' ? 'model' : 'user',
      parts: [{ text: message.content }]
    });
  }

  var requestBody = {
    contents: geminiContents
  };

  // Add system instruction if provided
  if (systemMessage) {
    requestBody.systemInstruction = {
      parts: [{ text: systemMessage }]
    };
  }

  // Add generation config for better control
  requestBody.generationConfig = {
    maxOutputTokens: 256,
    temperature: 1.0
  };

  console.log('Request body: ' + JSON.stringify(requestBody));
  xhr.send(JSON.stringify(requestBody));
}

// Send ready status to watch
function sendReadyStatus() {
  var apiKey = localStorage.getItem('api_key');
  var isReady = apiKey && apiKey.trim().length > 0 ? 1 : 0;

  console.log('Sending READY_STATUS: ' + isReady);
  Pebble.sendAppMessage({ 'READY_STATUS': isReady });
}

// Listen for app ready
Pebble.addEventListener('ready', function () {
  console.log('PebbleKit JS ready');
  sendReadyStatus();
});

// Listen for messages from watch
Pebble.addEventListener('appmessage', function (e) {
  console.log('Received message from watch');

  if (e.payload.REQUEST_CHAT) {
    var encoded = e.payload.REQUEST_CHAT;
    console.log('REQUEST_CHAT received: ' + encoded);

    var messages = parseConversation(encoded);
    console.log('Parsed ' + messages.length + ' messages');

    getClaudeResponse(messages);
  }
});

// Listen for when the configuration page is opened
Pebble.addEventListener('showConfiguration', function () {
  // Get existing settings
  var apiKey = localStorage.getItem('api_key') || '';
  var baseUrl = localStorage.getItem('base_url') || '';
  var model = localStorage.getItem('model') || '';
  var systemMessage = localStorage.getItem('system_message') || '';

  // Build configuration URL
  var url = "https://dinosan-kinde.github.io/client-ai/claude-for-pebble/config/";
  url += '?api_key=' + encodeURIComponent(apiKey);
  url += '&base_url=' + encodeURIComponent(baseUrl);
  url += '&model=' + encodeURIComponent(model);
  url += '&system_message=' + encodeURIComponent(systemMessage);

  console.log('Opening configuration page: ' + url);
  Pebble.openURL(url);
});

// Listen for when the configuration page is closed
Pebble.addEventListener('webviewclosed', function (e) {
  if (e && e.response) {
    var settings = JSON.parse(decodeURIComponent(e.response));
    console.log('Settings received: ' + JSON.stringify(settings));

    // Save or clear settings in local storage
    var keys = ['api_key', 'base_url', 'model', 'system_message'];
    keys.forEach(function (key) {
      if (settings[key] && settings[key].trim() !== '') {
        localStorage.setItem(key, settings[key]);
        console.log(key + ' saved');
      } else {
        localStorage.removeItem(key);
        console.log(key + ' cleared');
      }
    });

    // Send updated ready status to watch
    sendReadyStatus();
  }
});

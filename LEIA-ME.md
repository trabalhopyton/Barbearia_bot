# Barbearia_bot
// Chat bot da Barbearia HS Barbershop

const express = require('express');
const twilio = require('twilio');
const bodyParser = require('body-parser');


const accountSid = 'SEU_ACCOUNT_SID';
const authToken = 'SEU_AUTH_TOKEN';
const client = twilio(accountSid, authToken);

// Inicialização 
const app = express();
app.use(bodyParser.urlencoded({ extended: false }));

// Rota para receber as mensagens do WhatsApp
app.post('/webhook', (req, res) => {
    const message = req.body.Body.toLowerCase(); // A mensagem do usuário
    const sender = req.body.From; // Número de quem enviou

    let responseMessage = '';

    // Lagendamento
    if (message.includes('agendar')) {
        responseMessage = 'Qual data e hora você deseja agendar?';
    } else if (message.includes('segunda-feira')) {
        responseMessage = 'Seu agendamento foi marcado para segunda-feira às 15h.';
        // Aqui você poderia salvar o agendamento em um banco de dados
    } else {
        responseMessage = 'Oi! Eu sou o bot de agendamentos. Como posso ajudar?';
    }

    // Envia a resposta para o WhatsApp
    client.messages.create({
        body: responseMessage,
        from: 'whatsapp:+14155238886', // Número do Twilio para WhatsApp
        to: sender
    })
    .then(message => console.log(message.sid));

    // Resposta do servidor
    res.send('<Response></Response>');
});

// Rodando o servidor na porta 3000
app.listen(3000, () => {
    console.log('Servidor rodando na porta 3000');
});

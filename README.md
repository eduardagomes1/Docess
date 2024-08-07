import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.image.BufferedImage;
import java.io.IOException;
import javax.imageio.ImageIO;

public class Doces extends JFrame implements ActionListener {
    private JPanel candiesPanel;
    private JTextField[] quantidadeCampos;
    private JLabel[] docesLabel;
    private JLabel[] precoLabel;
    private JButton button; 
    private double[] precos = {3.50, 2.75, 1.00};

    public Doces() {
        setTitle("Loja de doces da Rafa");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout()); 
        quantidadeCampos = new JTextField[3];
        docesLabel = new JLabel[3];
        precoLabel = new JLabel[3];

        
        candiesPanel = new JPanel(new GridLayout(3, 3, 5, 5)); 
        String[] candyImagePaths = {"biscoito.jpeg", "Coracao.jpeg", "morango.jpeg"};
        for (int i = 0; i < 3; i++) {
            try {
                BufferedImage originalImage = ImageIO.read(getClass().getResource(candyImagePaths[i]));
                BufferedImage resizedImage = resizeImage(originalImage, 100, 100); 
                ImageIcon candyIcon = new ImageIcon(resizedImage);
                docesLabel[i] = new JLabel(candyIcon);
                candiesPanel.add(docesLabel[i]); 

                precoLabel[i] = new JLabel("R$" + precos[i]);
                precoLabel[i].setHorizontalAlignment(JLabel.RIGHT); // Alinha à direita
                candiesPanel.add(precoLabel[i]); 

                quantidadeCampos[i] = new JTextField(5);
                candiesPanel.add(quantidadeCampos[i]); 

            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        
        add(candiesPanel, BorderLayout.CENTER);

      
        button = new JButton("Pedir");
        button.addActionListener(this);
        JPanel buttonPanel = new JPanel(); 
        buttonPanel.add(button);
        
       
        add(buttonPanel, BorderLayout.SOUTH);

        pack();
        setLocationRelativeTo(null);
        setVisible(true);
    }

    private BufferedImage resizeImage(BufferedImage originalImage, int targetWidth, int targetHeight) {
        BufferedImage resizedImage = new BufferedImage(targetWidth, targetHeight, BufferedImage.TYPE_INT_ARGB);
        Graphics2D g = resizedImage.createGraphics();
        g.drawImage(originalImage, 0, 0, targetWidth, targetHeight, null);
        g.dispose();
        return resizedImage;
    }

    
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == button) { 
            double total = 0;
            for (int i = 0; i < 3; i++) {
                try {
                    int quantity = Integer.parseInt(quantidadeCampos[i].getText());
                    total += precos[i] * quantity;
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(this, "Por favor, insira um número válido.");
                    return; 
                }
            }
            JOptionPane.showMessageDialog(this, "Total da compra: R$" + total);
        }
    }

    public static void main(String[] args) {
        Doces Doces = new Doces();
    }
}

// Sean Ascheman
// N01180057
// 9/23/18
// Huffman Encoder

//IMPORTS
import java.util.Scanner;
import java.util.PriorityQueue;
import java.io.File;
import java.io.*;
import java.util.*;

public class Main{
   public static void main(String args[]) throws IOException{
      Scanner scan = new Scanner(System.in);
      
      Scanner fileScan = new Scanner(new File(args[0]));
      String myString = fileScan.nextLine();
      fileScan.close();
      
      Huffman myTree = new Huffman(myString);
      String[] encodedString = {};
      myTree.compress();
      
      String menuScan;
      System.out.println("Huffman Encoder Project 5");
      
      do{
         System.out.println();
         System.out.println("Press:");
         System.out.println("a: print tree");
         System.out.println("b: print code table");
         System.out.println("c: print encoded file");
         System.out.println("d: print decoded file (must encode first)");
         System.out.println("x: exit program");
         System.out.println();
         menuScan = scan.nextLine();
         
         switch(menuScan){
            case "a":
               myTree.printTree();
               break;
            case "b":
               myTree.printCode();
               break;
            case "c":
               myTree.printEncode(myTree.encodeFile());
               break;
            case "d":
               myTree.decodeFile(myTree.encodedString, myTree.root);
               break;
         }
      }while(!menuScan.equals("x"));
   }  
}

class Huffman{

   public Node root;
   private String[] st;
   private char[] input;
   private char[] useable = {'A','B','C','D','E','F','G'};
   private int[] freq = new int[256]; // Large 8 bit number
   public String[] encodedString = {};
   //-------------------------------------------------------
   public Huffman(String myString){
      this.input = myString.toCharArray();
   }
   //-------------------------------------------------------
   public void compress(){
      
      for(int i=0; i<input.length; i++){
         for(int j=0; j<useable.length; j++){
            if(input[i] == useable[j]) 
               freq[input[i]]++;          // REALLY COOL METHOD
         }                                // Increments each character    
       }                                   
      
      this.root = buildTree(freq);
      
      this.st = new String[256];
      buildCode(st, root, "");
   }
   //-------------------------------------------------------
   // Make a Tree
   private Node buildTree(int[] freq){
      PriorityQueue<Node> pq = new PriorityQueue<Node>();
      for(char i=0; i<256; i++)
         if(freq[i] > 0)
            pq.add(new Node(i, freq[i], null, null));
      
      while(pq.size() > 1){
         
         Node left = pq.remove();
         Node right = pq.remove();
         Node parent = new Node('-', left.freq + right.freq, left, right);
         pq.add(parent);
      }
      return pq.remove();
   }
   //-------------------------------------------------------
   // Make a whole bunch of codes for each character
   private void buildCode(String[] st, Node x, String s){
      if(!x.isLeaf()){
         buildCode(st, x.left, s+'0');
         buildCode(st, x.right, s+'1');
      }
      else{
         st[x.ch] = s;
      }
   }
   //-------------------------------------------------------
   public void printCode(){
      System.out.println("Char \t" + "Code");
      System.out.println("a \t\t" + st['A']);
      System.out.println("b \t\t" + st['B']);
      System.out.println("c \t\t" + st['C']);
      System.out.println("d \t\t" + st['D']);
      System.out.println("e \t\t" + st['E']);
      System.out.println("f \t\t" + st['F']);
      System.out.println("g \t\t" + st['G']);
   }
   //-------------------------------------------------------
   public void printTree(){
      Stack globalStack = new Stack();
      globalStack.push(root);
      int nBlanks = 32;
      boolean isRowEmpty = false;
      System.out.println
         ("...................................................................");
      while(isRowEmpty == false){
         Stack localStack = new Stack();
         isRowEmpty = true;
         
         for(int i=0; i<nBlanks; i++){
            System.out.print(' ');
         }
         
         while(globalStack.isEmpty() == false){
            Node temp = (Node)globalStack.pop();
            if(temp != null){
            System.out.print("{"+temp.ch+","+temp.freq+"}");
            localStack.push(temp.left);
            localStack.push(temp.right);
            
            if(temp.left != null || temp.right != null){
               isRowEmpty = false;
            }
            }
            else{
               System.out.print("--");
               localStack.push(null);
               localStack.push(null);
            }
            
            for(int j=0; j<nBlanks*2-1; j++)
               System.out.print(' ');           
         }
         System.out.println();
         nBlanks /= 2;
         while(localStack.isEmpty()==false){
            globalStack.push(localStack.pop());
         }
      }
      System.out.println
         ("...................................................................");
   }
   //-------------------------------------------------------
   public String[] encodeFile(){
      StringBuilder sb = new StringBuilder();
      for(int i=0; i<input.length; i++){
         if(input[i] == 'A' ||input[i] == 'B' ||input[i] == 'C' ||input[i] == 'D' ||input[i] == 'E' ||input[i] == 'F' ||input[i] == 'G')
            sb.append(st[input[i]]);
      }
      String[] encodedString = sb.toString().split("");      
      this.encodedString = encodedString;
      return encodedString;
   }
   //-------------------------------------------------------
   public void printEncode(String[] encodedString){
      for(int i=1; i<=encodedString.length; i++){
         System.out.print(encodedString[i-1]);
         if(i%8 == 0)
            System.out.print(" ");
         if(i%24 == 0)
            System.out.println();
      }
      System.out.println();
   }
   
   public void decodeFile(String[] myString, Node root){
      if(myString.length == 0){
         System.out.println("Encode a file first");
         return;
      }
      
      StringBuilder sb = new StringBuilder();
      Node current = root;
      
      for(int i=0; i<myString.length; i++){              // For each binary num
      
         if(current.left != null || current.right != null){

            if(myString[i].equals("0")){   // Go Left
               current = current.left;
            }
            else{                            // Go Right
               current = current.right;
            }
            if(current.left == null && current.right == null){
               System.out.print(current.ch);
               current = root;
            }
         }
      }
      System.out.println();
   }
}

////////////////////////////////////////////////////////////////
// Basic Node in Tree
class Node implements Comparable<Node>{
   public char ch;
   public int freq;
   public Node left;
   public Node right;
   
   public Node(char ch, int freq, Node left, Node right){   // Constructor
      this.ch = ch;
      this.freq = freq;
      this.left = left;
      this.right = right;
   }
   
   public boolean isLeaf(){                                 // is a leaf if children == null
      return (left == null) && (right == null);
   }
   
   public int compareTo(Node that){                         // compare two frequencies
      return this.freq - that.freq;
   }
}
////////////////////////////////////////////////////////////////

# Directed-Graph
This Digraph class, that provide such functions as: Reverse and Topological sort. Also there is I have simple functions as toString() addEdge() and etc.



package com.company;

import java.io.*;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Stack;

public class Digraph {
    private   int V;
    private   int E;
    private  Bag<Integer>[] adj;
    public Digraph reverseGraph;
    public boolean[] marked;
    public Stack<Integer> postOrder;
    public boolean hasCycle = false;
    public boolean[] onStack;

    public Digraph(int V) {
        this.V = V;
        adj = (Bag<Integer>[]) new Bag[V];
        for (int v = 0; v < V; v++) {
            adj[v] = new Bag<Integer>();
        }
    }

    public Digraph(In in) {
        try {
            this.V = in.readInt();
            adj = (Bag<Integer>[]) new Bag[V];
            for (int v = 0; v < V; v++) {
                adj[v] = new Bag<Integer>();
            }
            int E = in.readInt();
            for (int i = 0; i < E; i++) {
                int v = in.readInt();
                int w = in.readInt();
                addEdge(v, w);
            }
        } catch (NoSuchElementException e) {
            throw new IllegalArgumentException("invalid input format in Graph constructor", e);
        }
    }

    public void addEdge(int v, int w) {
        adj[v].add(w);
        E++;
    }

    public Iterable<Integer> adj(int v) {
        return adj[v];
    }

    public  int V() {
        return V;

    }

    public int E() {
        return E;
    }


    public Digraph reverse(){
        reverseGraph = new Digraph(V);
        for(int i = 0 ; i < V; i++){
            for(int w : adj[i]){
                reverseGraph.addEdge(w , i);
            }
        }
        return reverseGraph;
    }


    public String toString() {
        String current = V + " vertices, " + E + " edges " + "\n";
        for (int v = 0; v < V; v++) {
            current += v + ": ";
            for (int w : adj[v]) {
                current += w + " ";
            }
            current += "\n";
        }
        return current;
    }


    public int[] topologicalSort() {
        Digraph digraph = new Digraph(V);
        int[] newStack = new int[V];
        for(int i = 0 ;i< V; i++){
            for(int w : adj[i]){
                digraph.addEdge(i, w);
            }
        }
        postOrder = new Stack<Integer>();
        marked = new boolean[V];
        onStack = new boolean[V];
        for(int i = 0 ; i< V; i++){
            if(!marked[i]){
                dfs(digraph, i);
            }
        }
        if(hasCycle){
            System.out.println("There is a cycle");
        }
        else{
            for(int i = 0 ;i< V;  i++){
                newStack[i] += (postOrder.pop());
            }
        }
        return newStack;
    }



    public void dfs(Digraph G , int v){
        marked[v] = true;
        onStack[v] = true;
        for(int w: G.adj(v)){

            if(onStack[w]){
                hasCycle = true;
            }

            if(!marked[w]) {
                dfs(G, w);
            }
        }
        onStack[v] = false;
        postOrder.push(v);
    }

    public static void main(String[] args) throws FileNotFoundException{
        In in = new In("C:\\Users\\Admin\\Desktop\\check2.txt");
        Digraph G = new Digraph(in);
        System.out.println(G.toString());
        System.out.println();
        Digraph newGraph = G.reverse();
        System.out.println(newGraph.toString());
        System.out.println();
        int[] newStack = G.topologicalSort();
        for(int i = 0 ; i< G.V; i++){
            System.out.print(newStack[i] + " ");
        }

    }


}


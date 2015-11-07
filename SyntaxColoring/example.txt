//
//  ViewController.swift
//  SyntaxColoring
//
//  Created by Jesper Christensen on 07/11/2015.
//  Copyright © 2015 Jesper Christensen. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var webView: UIWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
    }
    
    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)

        if let filePath = NSBundle.mainBundle().pathForResource("example", ofType: "txt"),
            let sourceCode = try? NSString(contentsOfFile: filePath, encoding: NSUTF8StringEncoding) {
                let html = createHTML(sourceCode as String)
                
                let path = NSBundle.mainBundle().bundlePath
                let pathURL = NSURL(fileURLWithPath: path)
                
                webView.loadHTMLString(html, baseURL: pathURL)
        }
    }

    func createHTML(sourceCode: String) -> String {
        let cssLink = "<link rel='stylesheet' href='highlight.min.css'>"
        let jsScript = "<script src='highlight.min.js'></script>" +
                       "<script>hljs.initHighlightingOnLoad();</script>"
        let head = "<head>\(cssLink)\(jsScript)</head>";
        // TODO: XML escape the sourceCode
        let escapedSourceCode = sourceCode
        let body = "<body><pre><code>\(escapedSourceCode)</code></pre></body>";
        return "<DOCTYPE html><html>\(head)\(body)</html>"
    }
}


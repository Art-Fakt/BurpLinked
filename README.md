<p align="center">
  <a href="Images/BurpLinked.jpg">
    <img src="Images/BurpLinked.jpg" alt="BurpLinked" width="300" height="300"/>
  </a>
  <br/>
  <strong>BurpLinked</strong>
</p>

```
                                        LinkedIn Profile Extraction The Lazy Way
                                                     by @4rt3f4kt
```

LinkedIn recon the lazy way. 

## What it does

BurpLinked sits in the background while you browse LinkedIn via Burp and automatically extracts profile information from the API responses. 

TODO: For now there is just a problem with encoding specials french caracters when extracting Profiles data, Will be resolve that when I can.

## Installation

1. Download `BurpLinked.py`
2. Fire up Burp Suite
3. Go to **Extender** → **Extensions** → **Add**
4. Select **Python** and point it at the downloaded file
5. Start collecting souls by browsing LinkeDin Profiles

## Usage

### The Lazy Way (Recommended)
1. Enable "Auto-extract profiles" in the Settings tab
2. Browse LinkedIn through Burp's proxy
3. Do literally anything else while profiles get harvested
4. Export when you're done
5. If you want to erase all profiles with "Utilisateur LinkedIn" (for french case) as Name you can run this oneliner : 
`awk -F',' 'NR==1{print; next} {name=$1; gsub(/^ *"/,"",name); gsub(/" *$/,"",name); gsub(/^ +| +$/,"",name); if(name!="Utilisateur LinkedIn") print}' fichier.csv > new_fichier.csv`

If you only want to display all lines with "Utilisateur LinkedIn" as name, you can run:
`awk -F',' 'NR>1{n=$1; gsub(/^ *"/,"",n); gsub(/" *$/,"",n); gsub(/^ +| +$/,"",n); if(n=="Utilisateur Linkedin") print NR ":" $0}' fichier.csv`

Same thing for English version when you want to erase all profiles named "LinkedIn User" :
`awk -F',' 'NR==1{print; next} {name=$1; gsub(/^ *"/,"",name); gsub(/" *$/,"",name); gsub(/^ +| +$/,"",name); if(name!="LinkedIn User") print}' fichier.csv > new_fichier.csv`

If you only want to display all lines with "LinkedIn User" as name, you can run:
`awk -F',' 'NR>1{n=$1; gsub(/^ *"/,"",n); gsub(/" *$/,"",n); gsub(/^ +| +$/,"",n); if(n=="Linkedin User") print NR ":" $0}' fichier.csv`


### The Manual Way  
Right-click any LinkedIn response and select "Extract LinkedIn Profiles".

### Settings
- **Auto-extract** - Toggle automatic harvesting
- **Auto-save** - Dump profiles to CSV automatically
- **Debug logging** - See what's happening under the hood
- **Output directory** - Where your stolen data goes

## What gets extracted

- Full names (real ones, not "LinkedIn Member")
- Job titles and positions
- Geographic locations
- Profile URLs for follow-up stalking
- Premium badges and verification status
- Profile summaries when visible
- Timestamps for your records

## Output

Profiles get saved as CSV files with all the good stuff:

```csv
Name,Position,Location,Profile URL,Badge,Summary,Timestamp
"John Target","Senior Engineer","San Francisco, CA","https://linkedin.com/in/johntarget","Premium","Builds stuff...","2024-01-15 14:30:22"
```

## Target APIs

The extension monitors LinkedIn's GraphQL endpoints:
- `/voyager/api/graphql` - Main search results
- `/voyager/api/search` - Alternative search paths
- Any other LinkedIn API traffic it encounters

## Contributing

Found a bug? LinkedIn changed their API again? PRs welcome.

---
